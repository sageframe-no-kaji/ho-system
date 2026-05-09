# The Kanyō Learning Process

**A Case Study in Experiential AI-Assisted Learning**

---

## Overview

This document captures the learning methodology behind Kanyō, a real-time falcon detection system built by someone learning Python through structured, AI-assisted development. The project serves dual purposes: creating a functional wildlife monitoring system while developing deep understanding of software engineering practices.

---

## The Learner's Context

**Background:**
- 20+ years in education leadership (Chief Academic Officer at NuVu Studio, Director of Learning Design at Citizen Schools)
- Strong conceptual understanding of systems and architecture
- Learning Python as a practical skill, not just syntax memorization
- Values understanding *why* over just making things work

**Starting Point:**
- Familiar with basic programming concepts
- Limited Python experience
- Strong intuition for what makes good educational design
- Skeptical of "just ask AI to write code" approaches

**Goal:**
Not just to build a falcon detector, but to understand:
- How detection systems work
- How to architect maintainable code
- How to make informed technical decisions
- How to leverage AI as a thinking partner, not a code generator

---

## The Ho System: Structured Experiential Learning

### What is a "Ho"?

In Buddhism—particularly within Nichiren Buddhism—**Ho** carries multiple, layered meanings. At its core, it refers to **law**, **principle**, or **phenomena**, and appears in *Nam-myoho-renge-kyo* as a central pillar of the teaching.

On one level, **Ho** points to the **law or underlying principle** that governs reality: how things arise, change, and interrelate. On another, it refers to **phenomena**—the concrete, observable expressions of that law in lived experience.

In my own practice, **Ho** is also grounded in the body. It names a **step in walking meditation**: a single, intentional movement where awareness, action, and principle align. One step, taken fully.

**Significance**

- **Law / Principle:** The invisible structure that gives rise to form and meaning  
- **Phenomena:** The manifest world—experiences, sensations, actions, results  
- **Practice:** A discrete, mindful step that embodies the law rather than merely describing it  

A Ho is therefore both **abstract and immediate**: the rule and its expression, the teaching and the foot touching the ground. When applied to learning or building, a Ho becomes a bounded unit of attention in which principle is understood by being enacted.

A **Ho**, therefore, is a 2-3 hour focused learning session with:
- **Clear objective:** What you'll build
- **Concrete deliverable:** Working code + understanding
- **Learning checkpoints:** Concepts to grasp, not just tasks to complete
- **Documentation requirement:** Devlog capturing insights

**Key insight:** Hos are sized for cognitive load, not arbitrary feature boundaries. Each Ho should feel like "aha, I get it now" rather than "I copy-pasted a bunch of code."
### The Ho Structure

**Before the Ho:**
- Clear goal statement
- Success criteria (what works + what you understand)
- List of concepts you'll encounter

**During the Ho:**
- Build incrementally
- Test continuously
- Question decisions with AI
- Refine based on understanding

**After the Ho:**
- Document what you learned (not just what you built)
- Capture key insights and "gotchas"
- Note what still feels like "magic"
- Commit code with clear messages

### Example: Ho 2 (Falcon Vision)

**Objective:** Implement falcon detection with YOLOv8

**What was built:**
- Detection pipeline
- Event debouncing
- Clip extraction
- Hardware encoding

**What was learned:**
- How YOLO detection works conceptually
- Why debouncing matters for noisy signals
- Trade-offs between frame rate and CPU usage
- When to split modules (encoder.py refactor)

**Documentation produced:**
- 7-page devlog with code patterns, config values, and reflections
- "Confidence level: 4/5" - honest assessment of understanding
- Specific note: "ffmpeg still feels like incantation" - flagging gaps

---

## "Vibe Coding": AI as Thinking Partner

### What It's NOT

❌ **"Just ask AI to write the code"**
- Passive consumption of generated code
- Shallow understanding of what's happening
- Dependency on AI for every change
- "It works but I don't know why"

❌ **Traditional tutorial following**
- Step-by-step instructions without context
- No room for exploration or decisions
- One "right way" to do things
- No ownership of design choices

### What It IS

✅ **Collaborative problem-solving with AI as junior partner**

**The flow:**
1. **Human thinks through the problem** - "I need to detect when a falcon enters, but I'm getting spam from brief occlusions"
2. **Human proposes approach** - "What if we don't declare exit until 30 seconds of no detection?"
3. **AI executes implementation** - Writes the debounce logic
4. **Human reviews and questions** - "Why 30 seconds specifically? What if I make it 60?"
5. **Iterate based on understanding** - Test, tune, refine

**Key characteristic:** The human is making architectural decisions, the AI is handling syntax and boilerplate.

### Actual Example from Ho 2

**Human thought process:**
> "The detection is working but I'm getting Enter/Exit spam every few seconds. The falcon is just moving around, not actually leaving."

**Discussion with Claude:**
- Human: "Should I raise confidence threshold?"
- Claude: "That might help, but you'll also filter real detections. Consider debouncing instead."
- Human: "What's a reasonable timeout? 1 minute? 2 minutes?"
- Claude: "Looking at your gaps - they're all under 15 seconds. Try 30 or 60."
- Human: "Let's try 30. If that's too short, I'll tune it."

**Implementation by Copilot:**
```yaml
visit_merge_timeout: 30
```

**Result:** Human understands *why* debouncing works and *how* to tune it. AI wrote the config and code.

### Why This Works for Learning

**Traditional coding path:**
```
Idea → Google syntax → Stack Overflow → Copy code →
Debug syntax errors → More Googling → Finally works (maybe understand why)
```
**Time spent:** 70% syntax/googling, 30% thinking

**Vibe coding path:**
```
Idea → Discuss approach with AI → Review generated code →
Question decisions → Refine → Understand why it works
```
**Time spent:** 70% thinking/understanding, 30% syntax

**The difference:** Focus stays on concepts, not minutiae.

---

## The Dialectic: Human ↔ AI Collaboration

### Roles in the Partnership

**Human's job (Architect/Designer):**
- Define the problem space
- Make architectural decisions
- Question trade-offs
- Validate understanding
- Document learnings

**AI's job (Implementation Assistant):**
- Suggest approaches and patterns
- Write boilerplate code
- Handle syntax details
- Provide examples and explanations
- Catch common mistakes

**Critical distinction:** The human maintains the mental model of the system. The AI is a tool that accelerates implementation while the human thinks.

### Actual Dialogue Patterns

**Pattern 1: Clarifying Requirements**
```
Human: "I want to extract clips around events."
AI: "How much context before/after? Should close events merge?"
Human: "30s before entrance, 60s after. Yes, merge if within 3 minutes."
AI: [implements with those parameters]
```

**Pattern 2: Exploring Trade-offs**
```
Human: "Processing every frame is too slow."
AI: "You could process every Nth frame. Trade-off: speed vs missing brief appearances."
Human: "The falcon is slow-moving. Every 30th frame should work."
AI: [implements frame_interval=30]
```

**Pattern 3: Understanding Decisions**
```
Human: "Why did you split encoder.py out of clips.py?"
AI: "You'll need encoder detection for continuous recording too. Reusable module now."
Human: "Ah, good call. Let's do that refactor now while it's easy."
```

### When the Human Pushes Back

**Example from Ho 2:**
```
AI: "Try lowering confidence to 0.3 to catch more detections"
Human: "No - I want to catch FEWER detections, not more. The spam is from low-confidence flickers."
AI: "You're right! Raise confidence to 0.7 instead."
```

**This is critical:** The human maintains authority over decisions. The AI isn't an oracle, it's a tool that makes mistakes and needs correction.

---

## Tier-Based Understanding: Not Everything Needs Depth

### The Three Tiers

**Tier 3 (Deep Understanding):** How it works internally
- Example: "How does YOLO's neural network detect objects?"
- When needed: Rarely - only if optimizing or debugging ML issues

**Tier 2 (Functional Understanding):** How to use it and tune it
- Example: "What does confidence threshold control and how do I tune it?"
- When needed: YOUR DOMAIN LOGIC - the core of your system

**Tier 1 (Black Box):** It exists, trust it works
- Example: "ffmpeg encoding internals"
- When needed: Most dependencies and libraries

### Application to Kanyō

**Tier 1 (Black box - trust it):**
- YOLOv8 training process
- OpenCV video codec internals
- ffmpeg encoding algorithms
- YouTube streaming protocols

**Tier 2 (Functional - understand deeply):**
- Detection confidence thresholds
- Frame processing intervals
- Event debouncing logic
- Clip extraction timing
- Hardware encoder selection

**Tier 3 (Deep - optional):**
- How neural networks learn features
- Video compression mathematics
- (Nothing here yet - not needed)

**Philosophy:** Focus learning energy where it matters. You don't need to understand YOLO's architecture to use it effectively.

---

## Iterative Development: Build → Test → Understand → Refine

### The Cycle in Practice

**Ho 2 Example - Clip Extraction:**

**Iteration 1: Basic extraction**
```python
# Just extract clips with fixed timing
clip_start = event_time - 30
clip_end = event_time + 30
```
**Test:** Works! But clips are huge files.
**Learn:** Need compression.

**Iteration 2: Add compression**
```python
# Add ffmpeg encoding with libx264
ffmpeg -i input -c:v libx264 -crf 23 output
```
**Test:** Works! But slow (90 seconds per clip).
**Learn:** Software encoding is slow.

**Iteration 3: Hardware encoding**
```python
# Detect and use VideoToolbox on Mac
encoder = detect_hardware_encoder()
ffmpeg -c:v h264_videotoolbox ...
```
**Test:** Works! 5 seconds per clip.
**Learn:** Hardware encoding is WAY faster.

**Iteration 4: Refactor for reuse**
```python
# Split encoder detection to separate module
# Will need it for continuous recording later
```
**Test:** Still works, cleaner architecture.
**Learn:** When to refactor (before it's painful).

**Key insight:** Each iteration added understanding, not just features.

### Small Steps Compound

**Why this matters:**
- Can test at each step (know exactly where bugs are)
- Can stop at "good enough" (don't over-engineer)
- Can change direction easily (not committed to wrong path)
- Each step builds confidence

**Anti-pattern to avoid:**
"Build entire system, then test" = debugging nightmare

---

## Documentation as Learning Tool

### The Devlog Philosophy

**Purpose:** NOT a project log ("what I did today")
**Purpose:** A learning journal ("what I understand now")

**Structure:**
1. **What was built** - Concrete deliverables
2. **What was learned** - Concepts, patterns, insights
3. **Key decisions** - Why choices were made
4. **Gotchas** - What didn't work, what was surprising
5. **Reflection** - Honest assessment of understanding

### Example: Ho 2 Devlog Excerpt

**What makes it good:**

```markdown
### Key Insight
Split encoder.py out of clips.py because it will be reused
for continuous recording.

### Pattern Learned
def detect_hardware_encoder():
    # Try hardware encoders first
    # Fall back to software if none work

This is the "graceful degradation" pattern.

### Gotcha
YOLOv8 returns xyxy format for .xyxy accessor,
NOT xywh. Cost me 20 minutes of confusion.

### Confidence Level: 4/5
Understand the pipeline well. Some ffmpeg magic
still feels like incantation.
```

**Why this works:**
- Future you can remember *why* decisions were made
- Captures the learning, not just the code
- Honest about gaps (ffmpeg "incantation")
- Patterns documented for reuse

---

## Quality Through Process, Not Perfection

### The Linting Pipeline

**Tools used:**
- `black` - Code formatting
- `isort` - Import organization
- `flake8` - Style checking
- `mypy` - Type checking
- `pytest` - Testing

**Philosophy:** Let tools catch the mundane stuff so you can focus on logic.

**Workflow:**
```bash
# After coding session:
black src/ tests/           # Auto-format
isort src/ tests/           # Auto-organize imports
flake8 src/ tests/          # Catch style issues
mypy src/                   # Catch type errors
pytest tests/ -v            # Verify tests pass
```

**Learning moment:** First time running mypy, got 21 errors. Scary! But they were mostly:
- Configuration issues (fixed with mypy.ini)
- Missing type stubs (pip install types-PyYAML)
- One real bug (missing "merged" in Literal type)

**Lesson:** Tools find bugs *before* they cause problems. Trust the process.

### Test Coverage: Useful, Not Perfect

**Current status:** 54% coverage, 55 tests

**Philosophy:**
- Test the complex stuff (clip merging logic, event detection)
- Don't test obvious stuff (config file loading)
- Don't chase 100% coverage as a goal

**What gets tested:**
- Edge cases (clips at video boundaries)
- State machines (enter/exit logic)
- Merging logic (close events)

**What doesn't:**
- Simple getters/setters
- Configuration loading
- Logging statements

**Lesson:** Tests serve understanding, not metrics.

---

## Specific Learning Insights from Kanyō

### Insight 1: Debouncing is Everywhere

**Discovery:** Raw detections are noisy. Falcon detected 30x per second while present.

**Solution:** Don't declare "exit" until 30 seconds of no detection.

**Generalization:** This pattern applies to:
- Button presses (don't register multiple clicks)
- Sensor readings (ignore brief fluctuations)
- User input (wait for typing to finish)

**Why this matters:** Learned a fundamental CS pattern through practical need.

### Insight 2: Configuration Over Hard-Coding

**Discovery:** Kept tweaking values (confidence threshold, frame interval, timeout).

**Solution:** Move everything to config.yaml.

**Benefit:** Can tune without touching code.

**Lesson:** Separate "what" (logic) from "how much" (parameters).

### Insight 3: Refactor Early When Reuse is Clear

**Discovery:** "I'll need encoder detection for continuous recording too."

**Decision:** Split encoder.py out NOW, while it's easy.

**Result:** Clean, reusable module ready for Ho 3.

**Lesson:** Don't wait until refactoring is painful. Do it when you see the need.

### Insight 4: Trade-offs Are Everywhere

**Examples:**
- Frame rate vs CPU usage
- Confidence threshold vs missed detections
- Software vs hardware encoding (speed vs compatibility)
- Coverage vs development time

**Lesson:** There's no "right answer" - only trade-offs to understand and choose based on context.

### Insight 5: Test the Integration, Not Just the Units

**Discovery:** Clip extraction worked in isolation, but failed when integrated with detection.

**Problem:** Frame numbers were off by 1 because of 0-indexing.

**Solution:** Integration test that runs full pipeline.

**Lesson:** Components can work alone but fail together. Test the seams.

---

## The Philosophy: AI as Cognitive Bicycle

### The Bicycle Metaphor

**Without AI:**
- Walking speed: 3 mph
- Limited by physical capability
- Tiring over long distances

**With AI:**
- Cycling speed: 12 mph (4x faster)
- Human provides direction and power
- Machine amplifies human effort
- Still requires human skill to navigate

**Key point:** The bicycle doesn't ride itself. The human is still in control, still thinking, still deciding where to go.

### What AI Amplifies

**Amplified:**
- ✅ Implementation speed (4-10x faster)
- ✅ Ability to try alternatives (quick iteration)
- ✅ Learning from examples (AI provides patterns)
- ✅ Focus on concepts (less time on syntax)

**NOT amplified:**
- ❌ Architectural thinking (still human's job)
- ❌ Problem decomposition (still human's job)
- ❌ Understanding trade-offs (still human's job)
- ❌ System design (still human's job)

### The Danger: Passive Consumption

**What doesn't work:**
```
Human: "Build me a falcon detector"
AI: [generates 500 lines of code]
Human: "Cool!" [copies without reading]
```

**Why it fails:**
- No understanding of what was built
- Can't debug when it breaks
- Can't extend or modify
- Can't explain design choices
- Dependency on AI for every change

**What works:**
```
Human: "I need to detect falcons. Should I use YOLO or build custom?"
AI: "YOLO is pretrained on 'bird' class. Good starting point."
Human: "OK, how do I filter for high-confidence detections?"
AI: [explains confidence thresholds]
Human: "Got it. Set up detection with 0.5 threshold."
AI: [implements]
Human: [reviews code, tests, tunes threshold]
```

**Why it works:**
- Human makes each decision
- Human understands each component
- Human can debug and extend
- AI is accelerant, not substitute

---

## Comparison to Traditional Learning

### Traditional Bootcamp/Course

**Structure:**
- Fixed curriculum
- Follow tutorials step-by-step
- Build prescribed projects
- Graded on completion

**Outcome:**
- Can build what was taught
- Limited ability to adapt
- "How do I do X?" requires new tutorial
- Shallow understanding

### The Ho System with AI

**Structure:**
- Self-directed goals
- AI as implementation partner
- Build what you actually need
- Self-assessed on understanding

**Outcome:**
- Can design and build systems
- Strong ability to adapt
- "How do I do X?" → think through it with AI
- Deep understanding of decisions made

### Key Differences

| Aspect | Traditional | Ho + AI |
|--------|------------|---------|
| **Pacing** | Fixed by course | Self-directed |
| **Projects** | Prescribed | Self-motivated |
| **Learning** | Follow steps | Understand principles |
| **Assessment** | External grades | Self-evaluation |
| **Time on syntax** | 70% | 30% |
| **Time on concepts** | 30% | 70% |
| **Ownership** | "I completed the course" | "I built this system" |

---

## What's Being Learned (The Real Curriculum)

### Technical Skills

**Python:**
- Data structures (lists, dicts, dataclasses)
- Type hints and mypy
- Module organization
- Testing with pytest
- Async patterns (upcoming in Ho 3)

**Computer Vision:**
- Object detection with YOLO
- Frame processing pipelines
- Video encoding/decoding
- Real-time stream handling

**System Architecture:**
- Component separation
- State machines
- Event-driven design
- Configuration management
- Error handling

**DevOps:**
- Docker containerization (upcoming)
- Deployment strategies
- Process monitoring
- Log management

### Meta-Skills (More Important)

**Problem Decomposition:**
- Breaking big problems into Hos
- Identifying dependencies
- Sequencing work logically

**Trade-off Analysis:**
- Speed vs accuracy
- Simplicity vs features
- Now vs later
- Perfect vs good enough

**Iterative Development:**
- Build → test → learn → refine
- Small steps compound
- Fail fast, recover faster

**Collaborative AI Use:**
- When to lean on AI
- When to push back
- How to maintain understanding
- Validation vs blind trust

**Documentation for Learning:**
- Capturing decisions
- Recording insights
- Honest self-assessment
- Building knowledge base

---

## The Outcome: What Success Looks Like

### After 3 Hos (Current State)

**Can do:**
- ✅ Architect a detection system
- ✅ Make informed trade-off decisions
- ✅ Debug issues methodically
- ✅ Extend system with new features
- ✅ Explain how components work
- ✅ Use AI as effective partner

**Can't do yet:**
- ❌ Everything from scratch without AI (that's OK!)
- ❌ Optimize ML models (Tier 3 - not needed)
- ❌ Design distributed systems (haven't learned yet)

**Confidence level:** 4/5 overall
- Strong on architecture and decisions
- Some implementation details still fuzzy
- Comfortable with not knowing everything

### After 10 Hos (Projected)

**Will be able to:**
- Design and deploy production systems
- Make architectural decisions confidently
- Use multiple ML models effectively
- Handle real-time data pipelines
- Debug complex integration issues
- Teach others the concepts learned

**Still won't know:**
- Deep ML theory (don't need it)
- Every Python library (nobody does)
- How to do everything without AI (that's the point!)

---

## Reflections on the Process

### What's Working

**The Ho structure:**
- Right size for focused learning
- Clear goals prevent scope creep
- Deliverables provide satisfaction
- Documentation cements understanding

**AI as partner:**
- Accelerates implementation 4-10x
- Keeps focus on concepts
- Enables rapid iteration
- Provides examples and explanations

**Tier-based understanding:**
- Don't get stuck on irrelevant details
- Focus energy where it matters
- Comfortable with black boxes

**Quality tooling:**
- Catches mistakes early
- Professional-grade output
- Builds good habits

### What's Challenging

**Trusting the process:**
- Temptation to dive deeper than needed
- FOMO on "should I understand this better?"
- Resisting perfectionism

**AI limitations:**
- Sometimes suggests wrong approaches
- Need to verify and validate
- Can't replace architectural thinking

**Self-discipline:**
- Easy to let sessions expand beyond 2-3 hours
- Documentation requires discipline
- Testing feels like "extra work" (but isn't)

### What's Surprising

**How fast real progress happens:**
- 3 Hos = working system
- Each Ho builds meaningfully
- Compounding knowledge

**How much understanding remains:**
- Feared AI would create black boxes
- Actually understand more than traditional learning
- Can explain decisions and trade-offs

**How natural the collaboration feels:**
- AI as "junior developer" metaphor works
- Dialectic (question/discuss/refine) is powerful
- Human stays firmly in control

---

## Advice for Others

### If You Want to Learn This Way

**Start with:**
1. Pick a real project you care about (not a tutorial)
2. Break it into 2-3 hour chunks (Hos)
3. Use AI as thinking partner, not code generator
4. Document your understanding, not just your code
5. Test and iterate continuously

**Avoid:**
- ❌ "Build everything for me" prompts
- ❌ Copy-pasting without understanding
- ❌ Skipping documentation
- ❌ Trying to learn everything at once
- ❌ Perfectionism over progress

**Embrace:**
- ✅ Small, tested increments
- ✅ Questioning AI suggestions
- ✅ Honest self-assessment
- ✅ "Good enough" over perfect
- ✅ Learning by building

### If You're Skeptical of AI-Assisted Learning

**Valid concerns:**
- "Will I actually learn or just copy?"
- "Am I becoming dependent on AI?"
- "Is this real engineering?"

**Responses:**
- The devlog proves understanding (can't fake insights)
- Dependency test: Can you explain and extend the code? (Yes)
- Real engineering is making informed decisions (AI helps, doesn't decide)

**Try this:**
- Build something small with AI assistance
- Document what you learned
- Try to extend it without AI
- Assess your understanding honestly

**You'll find:** You learned more than you expected, faster than traditional methods, with better outcomes.

---

## Conclusion: A New Model for Technical Learning

The Kanyō process demonstrates that AI-assisted learning can:
- **Accelerate skill acquisition** without sacrificing depth
- **Maintain human agency** in design and decision-making
- **Focus energy on concepts** rather than syntax
- **Produce working systems** while building understanding
- **Scale to complex projects** through iterative structure

The key insight: **AI is a cognitive bicycle, not a replacement for thinking.**

The human provides:
- Direction (what to build)
- Decisions (architectural choices)
- Validation (does it work? do I understand?)
- Learning (capturing insights)

The AI provides:
- Implementation (syntax and boilerplate)
- Examples (patterns and approaches)
- Acceleration (4-10x faster iteration)
- Explanation (how things work)

Together, this creates a **learning system that is greater than the sum of its parts**: faster than traditional learning, deeper than tutorial-following, more sustainable than memorization.

The future of technical education isn't "learn to code without AI" or "let AI code for you." It's **"learn to think in systems and use AI as a force multiplier."**

Kanyō is proving that model works.

---

**Document version:** 1.0 (Ho 2 complete)
**Next update:** After Ho 5 (with more patterns and insights)
**Author:** Tyro (learning in public)
**Date:** December 17, 2024
