# Kanyō YOLO Fine-Tuning: Complete Planning Guide

## Executive Summary

**Goal:** Train a custom YOLO model to reliably detect falcons in the Harvard and NSW nest cameras, especially in challenging conditions (IR/night mode, unusual poses, doorway silhouettes).

**Current Problem:** The generic YOLOv8n model (trained on COCO dataset) inconsistently detects falcons, classifying them as "bear", "elephant", "cat", or "person" with low confidence (0.20-0.40). This causes:
- False departures (bird present but not detected for 90s)
- False arrivals (noise detected as bird)
- Unreliable notifications

**Solution:** Fine-tune YOLO on YOUR specific camera footage so it learns "this exact thing is a falcon in THIS setup."

---

## Part 1: Understanding the Options

### Option A: Object Detection (Bounding Boxes) ⭐ RECOMMENDED

**What it is:** Train YOLO to draw boxes around falcons and output "falcon: 0.95"

**Pros:**
- Works with your existing codebase (no code changes needed)
- Can detect multiple birds
- Provides location information (useful for future features)
- You're already using object detection, so it's a drop-in replacement

**Cons:**
- Requires drawing bounding boxes around birds in each image
- Slightly more labeling effort

**Effort:** ~2-4 hours labeling, 30 min training

---

### Option B: Binary Image Classification

**What it is:** Train a simple model to answer "Is there a bird in this frame? Yes/No"

**Pros:**
- Much faster to label (just sort images into folders)
- Simpler model, faster inference
- Good for fixed cameras where bird is always in similar region

**Cons:**
- Requires code changes to replace YOLO detection
- Can't detect multiple birds
- No location information
- Less flexible for future camera angles

**Effort:** ~1 hour labeling, 15 min training, ~2 hours code changes

---

### My Recommendation: **Option A (Object Detection)**

Since your codebase already uses YOLO object detection, fine-tuning is a drop-in replacement. You just swap the model file and everything else stays the same.

---

## Part 2: Labeling Tools Comparison

### Roboflow (Cloud) ⭐ RECOMMENDED FOR YOU

**Website:** https://roboflow.com

**What it is:** Web-based annotation platform with free tier

**Pros:**
- No setup - works in browser
- Built-in AI labeling assistance (SAM-2 helps draw boxes)
- Exports directly to YOLOv8 format
- Free tier: 10,000 images, 3 projects
- Can train models directly on their platform (optional)
- Dataset health checks (finds problems automatically)

**Cons:**
- Images uploaded to their cloud
- Limited features on free tier
- Requires internet

**Best for:** Quick projects, beginners, small datasets (<1000 images)

---

### Label Studio (Self-Hosted)

**Website:** https://labelstud.io

**What it is:** Open-source annotation tool you run locally

**Pros:**
- Free, no limits
- Data stays on your machine
- Highly customizable
- Supports many export formats

**Cons:**
- Requires setup (`pip install label-studio`)
- No AI assistance for drawing boxes
- More manual work

**Best for:** Privacy-sensitive projects, large datasets, teams

---

### CVAT (Self-Hosted)

**Website:** https://cvat.ai

**What it is:** Intel's open-source annotation tool

**Pros:**
- Excellent for video annotation
- Frame interpolation (label one frame, auto-labels similar frames)
- Free, self-hosted

**Cons:**
- Requires Docker setup
- Heavier resource usage
- Overkill for still images

**Best for:** Video annotation, large teams

---

### My Recommendation: **Roboflow**

For your use case (50-100 images, quick turnaround, drop-in model replacement), Roboflow is the fastest path. Upload images, draw boxes, export, train.

---

## Part 3: The Complete Workflow

### Step 1: Collect Images (1-2 hours)

**Target: 50-100 images total**

You need images covering ALL the failure cases:

| Category | Count | Source | Priority |
|----------|-------|--------|----------|
| Bird in doorway (IR) | 15-20 | NSW old camera clips | HIGH |
| Bird sleeping/tucked | 10-15 | NSW/Harvard clips | HIGH |
| Bird close-up (filling frame) | 10-15 | Harvard clips | HIGH |
| Bird normal pose (IR) | 10-15 | Both cameras | MEDIUM |
| Bird normal pose (daytime) | 10-15 | Both cameras | MEDIUM |
| Empty nest (IR) | 15-20 | Both cameras | HIGH |
| Empty nest (daytime) | 10-15 | Both cameras | MEDIUM |

**How to collect:**

1. **From existing clips:**
   ```bash
   # Extract frame from a video at specific time
   ffmpeg -ss 00:00:10 -i clip.mp4 -frames:v 1 frame_001.jpg

   # Extract multiple frames (every 30 seconds)
   ffmpeg -i clip.mp4 -vf "fps=1/30" frames_%03d.jpg
   ```

2. **From live stream:**
   - Take screenshots while watching
   - Save as PNG or JPG

3. **From existing thumbnails:**
   ```bash
   # Copy all existing thumbnails
   cp /opt/services/kanyo-*/clips/**/*.jpg ~/training_images/
   ```

**File naming convention:**
```
falcon_ir_doorway_001.jpg
falcon_ir_sleeping_001.jpg
falcon_day_normal_001.jpg
empty_ir_001.jpg
empty_day_001.jpg
```

---

### Step 2: Label Images with Roboflow (1-2 hours)

#### 2a. Create Account & Project

1. Go to https://roboflow.com and sign up (free)
2. Click "Create Project"
3. Settings:
   - Project Name: `kanyo-falcon-detection`
   - Project Type: **Object Detection**
   - Annotation Group: `falcon`

#### 2b. Upload Images

1. Click "Upload" → drag and drop all your images
2. Wait for upload to complete

#### 2c. Label Images

For each image with a falcon:

1. Click the image to open annotation editor
2. Select the "falcon" class
3. Draw a bounding box around the falcon
   - Include the whole bird (head to tail, wingtips)
   - Don't include too much background
4. Click "Save" or press Enter
5. Move to next image

**Labeling tips:**
- Use keyboard shortcuts (faster)
- For tricky IR images, draw box around where YOU see the bird
- For empty images, just save without any boxes
- Be consistent with box tightness

**AI Assist (optional):**
- Click "Smart Polygon" to let AI suggest boundaries
- Review and adjust as needed

#### 2d. Review & Generate Dataset

1. After labeling all images, click "Generate"
2. Settings:
   - Train/Valid/Test split: 70% / 20% / 10%
   - Preprocessing: Auto-Orient, Resize to 640x640
   - Augmentation:
     - Flip Horizontal: Yes
     - Brightness: ±15%
     - Blur: Up to 1px
     - (Skip rotation - birds shouldn't be upside down)
3. Click "Generate"

#### 2e. Export Dataset

1. Click "Download Dataset"
2. Format: **YOLOv8**
3. Download the ZIP file
4. Unzip to get folder structure:
   ```
   dataset/
   ├── data.yaml
   ├── train/
   │   ├── images/
   │   └── labels/
   └── valid/
       ├── images/
       └── labels/
   ```

---

### Step 3: Train the Model (30-60 minutes)

#### 3a. Setup on Your GPU Server (shingan)

```bash
# SSH to your server
ssh shingan

# Create training directory
mkdir -p ~/yolo-training
cd ~/yolo-training

# Copy/upload dataset (from your Mac)
# Option 1: scp
scp -r ~/Downloads/dataset.zip shingan:~/yolo-training/

# Option 2: If dataset is on Roboflow, download directly
curl -L "https://universe.roboflow.com/ds/YOUR_DATASET_LINK" > dataset.zip

# Unzip
unzip dataset.zip
```

#### 3b. Create Training Script

```python
# train_falcon.py
from ultralytics import YOLO

# Load pretrained model (transfer learning)
model = YOLO('yolov8n.pt')  # Start from pretrained weights

# Train on your dataset
results = model.train(
    data='dataset/data.yaml',  # Path to your data.yaml
    epochs=50,                  # 50 is usually enough for fine-tuning
    imgsz=640,                  # Image size
    batch=16,                   # Adjust based on GPU memory
    device=0,                   # GPU 0
    patience=10,                # Stop if no improvement for 10 epochs
    name='falcon_detector',     # Run name

    # Fine-tuning specific settings
    lr0=0.001,                  # Lower learning rate for fine-tuning
    lrf=0.01,                   # Final learning rate fraction

    # Augmentation (already applied in Roboflow, but can add more)
    flipud=0.0,                 # No vertical flip (birds aren't upside down)
    mosaic=0.5,                 # Mosaic augmentation
)

print(f"Training complete! Best model: runs/detect/falcon_detector/weights/best.pt")
```

#### 3c. Run Training

```bash
# Activate your Python environment
source /path/to/venv/bin/activate  # or conda activate

# Install ultralytics if needed
pip install ultralytics

# Run training
python train_falcon.py
```

**What you'll see:**
```
Epoch    GPU_mem   box_loss   cls_loss   dfl_loss  Instances       Size
  1/50      3.2G     1.234      0.876      1.567         12        640
  2/50      3.2G     1.156      0.823      1.489         12        640
  ...
 50/50      3.2G     0.423      0.234      0.567         12        640

Results saved to runs/detect/falcon_detector
```

#### 3d. Training Output

After training, you'll have:
```
runs/detect/falcon_detector/
├── weights/
│   ├── best.pt      ← YOUR FINE-TUNED MODEL
│   └── last.pt
├── results.png      ← Training curves
├── confusion_matrix.png
└── val_batch0_pred.jpg  ← Validation predictions
```

---

### Step 4: Deploy the Model (15 minutes)

#### 4a. Copy Model to Kanyō

```bash
# Copy the best model to your kanyo models directory
cp ~/yolo-training/runs/detect/falcon_detector/weights/best.pt \
   /opt/services/kanyo-nsw/models/falcon_v1.pt

cp ~/yolo-training/runs/detect/falcon_detector/weights/best.pt \
   /opt/services/kanyo-harvard/models/falcon_v1.pt
```

#### 4b. Update Configs

Edit `/opt/services/kanyo-nsw/config.yaml`:
```yaml
# Change from:
model_path: "models/yolov8n.pt"

# To:
model_path: "models/falcon_v1.pt"

# Also update class detection
detect_any_animal: false  # Now we only want "falcon" class
# Remove or comment out animal_classes - not needed anymore
```

Repeat for Harvard config.

#### 4c. Restart Containers

```bash
cd /opt/services/kanyo-admin
docker compose restart nsw-gpu harvard-gpu
```

#### 4d. Verify It's Working

```bash
# Watch logs
tail -f /opt/services/kanyo-nsw/logs/kanyo.log | grep -E "YOLO|Falcon|detected"

# You should see:
# YOLO found 1 objects: falcon(0):0.92
# Falcon detected: confidence=0.923
```

---

## Part 4: Troubleshooting & Iteration

### If Detection is Still Flaky

1. **Collect more images of failure cases**
   - Look at clips where it failed
   - Add those specific poses/conditions to training data

2. **Check your labels**
   - Were boxes tight enough?
   - Did you include all the tricky cases?

3. **Try more epochs**
   - Increase to 100 epochs
   - Watch for overfitting (val loss going up while train loss goes down)

4. **Try a bigger model**
   - Use `yolov8s.pt` (small) instead of `yolov8n.pt` (nano)
   - Slower but more accurate

### If You Get False Positives (detecting falcon when empty)

1. **Add more "empty nest" images to training**
   - Especially empty IR images
   - Label them WITHOUT any boxes

2. **Raise confidence threshold**
   - Start at 0.5 for your custom model
   - Adjust based on results

### Model Versioning

Keep track of your models:
```
models/
├── yolov8n.pt           # Original generic model
├── falcon_v1.pt         # First fine-tuned model
├── falcon_v2.pt         # After more training data
└── falcon_v3_harvard.pt # Camera-specific if needed
```

---

## Part 5: Advanced Options (Future)

### Camera-Specific Models

If Harvard and NSW have very different characteristics:
- Train separate models for each
- Configure `model_path` per stream

### Continuous Improvement

1. When you notice a detection failure:
   - Screenshot the frame
   - Add to training dataset
   - Retrain periodically (monthly?)

2. Use Roboflow's model monitoring:
   - Track confidence distributions over time
   - Get alerts when model performance degrades

### Alternative: Train on Roboflow Directly

Roboflow can train models for you (no local GPU needed):
1. After labeling, click "Train" instead of "Download"
2. Wait ~30 min for cloud training
3. Download the trained model

Free tier includes 3 training credits.

---

## Appendix A: Dataset YAML Format

If you need to create manually:

```yaml
# data.yaml
path: /home/user/dataset  # Dataset root directory
train: train/images       # Train images (relative to path)
val: valid/images         # Validation images

# Classes
names:
  0: falcon
```

---

## Appendix B: YOLO Label Format

Each image needs a matching `.txt` file with same name:

```
# falcon_001.txt
# class_id center_x center_y width height (all normalized 0-1)
0 0.512 0.487 0.234 0.312
```

If image has no falcon, the txt file should be empty or not exist.

---

## Appendix C: Quick Reference Commands

```bash
# Extract frames from video
ffmpeg -i input.mp4 -vf "fps=1" frames_%04d.jpg

# Train model
yolo train data=data.yaml model=yolov8n.pt epochs=50 imgsz=640

# Test model on images
yolo predict model=best.pt source=test_images/

# Export model to different format
yolo export model=best.pt format=onnx

# Validate model
yolo val model=best.pt data=data.yaml
```

---

## Summary: Minimum Viable Training

**Time investment: 3-4 hours total**

1. **Collect 50-100 images** (1-2 hours)
   - Focus on failure cases
   - Include empty nest images

2. **Label in Roboflow** (1-2 hours)
   - Draw boxes around falcons
   - Export as YOLOv8 format

3. **Train on your GPU** (30 min)
   ```python
   from ultralytics import YOLO
   model = YOLO('yolov8n.pt')
   model.train(data='data.yaml', epochs=50)
   ```

4. **Deploy** (15 min)
   - Copy `best.pt` to kanyo models folder
   - Update config to use new model
   - Restart containers

**That's it.** Your model will now detect "falcon" with 0.90+ confidence instead of "bear" with 0.21 confidence.
