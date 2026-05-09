# Ho 0.5: Tool Mastery

## Learning the Development Workflow

**Duration:** 1-1.5 hours
**Goal:** Install and understand Claude Code, Git basics, and establish efficient workflow patterns
**Deliverable:** Working development environment + confidence with tools

---

## Why This Ho Matters

Before we build falcon detection, we need to master our tools. This ho is low-stakes practice that will make every subsequent ho smoother. Think of it as sharpening your saw before cutting wood.

**You'll learn:**

- How to use Claude Code effectively
- Basic Git commands that matter
- When to use which tool
- How to iterate with AI assistance

---

## Prerequisites Checklist

Before starting, verify you have:

- [ ] **Python 3.10+** installed

  ```bash
  python3 --version
  # Should show 3.10 or higher
  ```

- [ ] **Git** installed

  ```bash
  git --version
  # Should show any version
  ```

- [ ] **VSCode** installed and can open from terminal

  ```bash
  code --version
  # Should show version number
  ```

- [ ] **GitHub account** created (you'll need this)

- [ ] **Claude Pro subscription** active

- [ ] **GitHub Copilot** working in VSCode (via Education account)

**If anything is missing:** Install before proceeding. Need help? Come back to Claude.ai chat.

---

## Part 1: Installing Claude Code (15 minutes)

### Step 1: Install Claude Code CLI

**On macOS/Linux:**

```bash
# Install via curl
curl -sS https://storage.googleapis.com/anthropic-artifacts/claude-code/install.sh | bash

# Or if you prefer, via npm
brew install claude-code
```

### Step 2: Authenticate

```bash
# This will open browser to authenticate with your Claude Pro account
claude auth login
```

Follow the prompts to connect your Claude Pro subscription.

### Step 3: Verify Installation

```bash
# Check it works
claude --version

# Try help command
claude --help
```

**Expected output:** Version number and help text

### Step 4: Test Basic Usage

Create a test directory and try your first command:

```bash
# Create test space
mkdir ~/claude-code-test
cd ~/claude-code-test

# First Claude Code command!
claude "Create a Python file called hello.py that prints 'Hello from Claude Code'"
```

**What should happen:**

1. Claude Code analyzes the request
2. Shows you what it plans to do
3. Creates the file
4. You can review and approve

**Try running it:**

```bash
python3 hello.py
```

**Success criteria:** You see "Hello from Claude Code"

---

## Part 2: Understanding Claude Code Workflow (20 minutes)

### How Claude Code Works

**The conversation pattern:**

1. You give a command
2. Claude proposes changes
3. You review (it shows a diff)
4. You approve/reject/modify
5. Claude executes

### Practice Exercise: Iterative Development

Let's practice the back-and-forth:

```bash
# Still in ~/claude-code-test

# Step 1: Create a simple calculator
claude "Create calculator.py with functions for add, subtract, multiply, divide"

# Step 2: Add features
claude-code "Add error handling for division by zero to calculator.py"

# Step 3: Add tests
claude-code "Create test_calculator.py with pytest tests for all functions"
```

**Notice:**

- Each command builds on previous work
- Claude Code remembers context within a session
- You can iterate: "Actually, change that to..." works!

### Model Selection Practice

Try the same task with different models:

```bash
# With Sonnet (default, balanced)
claude-code "Create a function that calculates fibonacci numbers"

# With Opus (more sophisticated)
claude-code --model claude-opus-4 "Create a function that calculates fibonacci numbers with memoization and explain the optimization"

# With Haiku (quick and simple)
claude-code --model claude-haiku-4 "Add a comment to fibonacci explaining what it does"
```

**Observe the differences:**

- Opus: More detailed, better explanations
- Sonnet: Good balance
- Haiku: Fast, concise, good for simple tasks

### When to Use Each Model

**Opus (`--model claude-opus-4`):**

- Learning new concepts
- Complex algorithms
- Need detailed explanations
- Architectural decisions

**Sonnet (default):**

- Standard implementation
- Most day-to-day coding
- Good enough for 80% of tasks

**Haiku (`--model claude-haiku-4`):**

- Quick fixes
- Simple edits
- "Change X to Y"
- Documentation updates

---

## Part 3: Git Basics (25 minutes)

### Create Your First Real Repo

```bash
# Navigate to where you want your projects
cd ~/Documents  # or wherever you keep code

# Create kanyo directory
mkdir kanyo
cd kanyo

# Initialize git
git init

# Create a basic README
echo "# Kanyō (観鷹)" > README.md
echo "Contemplating Falcons - Peregrine Detection Project" >> README.md
```

### The Essential Git Commands

**The workflow you'll use constantly:**

```bash
# 1. Check what's changed
git status

# 2. Add files to staging
git add README.md
# Or add everything:
git add .

# 3. Commit with message
git commit -m "Initial commit: project setup"

# 4. Check history
git log --oneline
```

### Connect to GitHub

**On GitHub.com:**

1. Click "New Repository"
2. Name it: `kanyo`
3. Make it public (or private if you prefer)
4. **Don't** initialize with README (we already have one)
5. Copy the repository URL

**In your terminal:**

```bash
# Add remote (replace USERNAME with yours)
git remote add origin git@github.com:USERNAME/kanyo.git

# Push to GitHub
git branch -M main
git push -u origin main
```

**Troubleshooting SSH:**
If push fails with "Permission denied", you need to set up SSH keys:

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"
# Press enter to accept defaults

# Copy public key
cat ~/.ssh/id_ed25519.pub
# Copy this output

# Add to GitHub:
# GitHub.com → Settings → SSH and GPG keys → New SSH key
# Paste the key, save

# Try push again
git push -u origin main
```

### Practice Git Workflow

```bash
# Make a change
echo "Origin story: Conversation with Claudia Goldin" >> README.md

# See what changed
git status
git diff

# Stage and commit
git add README.md
git commit -m "Add origin story"

# Push to GitHub
git push
```

**Check GitHub:** Refresh your repo page - you should see the changes!

---

## Part 4: VSCode Integration (15 minutes)

### Open Your Project

```bash
# From kanyo directory
code .
```

This opens VSCode with your project.

### Verify GitHub Copilot

1. Create a new file: `test.py`
2. Start typing: `def calculate_area_of_circle(`
3. Copilot should suggest the rest
4. Press Tab to accept

**If Copilot isn't working:**

- Click Copilot icon in VSCode status bar
- Sign in with GitHub account
- Verify Education pack is active

### VSCode Git Integration

**Try these:**

1. Make a change to README.md in VSCode
2. Click Source Control icon (left sidebar)
3. See your changes
4. Stage by clicking "+"
5. Write commit message
6. Click ✓ to commit
7. Click "..." → Push

**This is the GUI version of git commands!**

### File Explorer Practice

Create this structure in VSCode:

```
kanyo/
├── README.md
├── docs/
│   └── notes.md
├── devlog/
│   └── ho-0.5-tool-mastery.md (this file!)
└── src/
    └── __init__.py
```

**How:**

1. Use VSCode file explorer
2. Right-click → New Folder
3. Create the structure
4. Add files

**Then commit everything:**

```bash
git add .
git commit -m "Ho 0.5: Create project structure"
git push
```

---

## Part 5: Practice Mission (15 minutes)

### Mini-Project: Weather Reporter

Let's combine everything you've learned:

**The challenge:** Use Claude Code to create a simple weather script, then manage it with Git.

```bash
# In your kanyo/src directory
cd ~/path/to/kanyo/src

# Use Claude Code to build it
claude-code "Create weather.py that takes a city name as input and prints a mock weather report with temperature, conditions, and a 3-day forecast. Make it colorful with emoji."
```

**After Claude Code creates it:**

```bash
# Test it works
python weather.py

# If you want changes:
claude-code "Add humidity and wind speed to the weather report"

# When happy, commit it
git add weather.py
git commit -m "Ho 0.5: Add weather reporter practice script"
git push
```

### Bonus Challenge (optional)

Try iterating with different approaches:

```bash
# Ask Claude Code to refactor
claude-code "Refactor weather.py to use a Weather class"

# Or add features
claude-code "Add argument parsing so I can run: python weather.py --city Boston"
```

**Practice the cycle:**

- Request with Claude Code
- Test manually
- Iterate if needed
- Commit when working
- Push to GitHub

---

## Part 6: Document Your Learning (10 minutes)

### Create Your Ho 0.5 Devlog

In `kanyo/devlog/ho-0.5-tool-mastery.md`, write:

```markdown
# Ho 0.5: Tool Mastery

**Date:** [today's date]
**Duration:** [actual time spent]
**Status:** Complete ✓

## What I Learned

### Claude Code

- [What worked well]
- [What confused me]
- [Favorite feature]

### Git

- [Commands I understand now]
- [What I need more practice with]

### VSCode

- [What I configured]
- [Useful shortcuts I learned]

## Challenges & Solutions

[Any problems you hit and how you solved them]

## Key Takeaways

1. [Most important lesson]
2. [Something surprising]
3. [What I'm excited to use in Ho 1]

## Next Steps

Ready for Ho 1: Git Good - proper repository setup for kanyo detection system.
```

**Commit your devlog:**

```bash
git add devlog/ho-0.5-tool-mastery.md
git commit -m "Ho 0.5: Complete tool mastery devlog"
git push
```

---

## Ho 0.5 Completion Checklist

Before moving to Ho 1, verify:

- [x] Claude Code installed and authenticated
- [ ] Successfully created files with all three models (Opus, Sonnet, Haiku)
- [ ] Git repo created and pushed to GitHub
- [ ] Can make changes → stage → commit → push
- [ ] VSCode opens project, Copilot works
- [ ] Weather reporter script created and committed
- [ ] Devlog written and pushed
- [ ] Comfortable with the workflow cycle

---

## Troubleshooting

### Claude Code Issues

**"Authentication failed"**

- Run `claude-code auth logout` then `claude-code auth login`
- Verify Claude Pro subscription is active

**"Command not found"**

- Check installation path: `which claude-code`
- Add to PATH if needed
- Try closing and reopening terminal

**"Rate limit exceeded"**

- You've used your Claude Pro quota
- Wait for 5-hour rolling window to reset
- Check usage: You should see message count in responses

### Git Issues

**"Permission denied (publickey)"**

- Need to set up SSH keys (see Part 3)
- Or use HTTPS instead of SSH URL

**"fatal: remote origin already exists"**

- Run `git remote remove origin`
- Then add again with correct URL

**"Your branch is ahead of 'origin/main'"**

- You have local commits not pushed
- Run `git push` to sync

### VSCode Issues

**Copilot not working**

- Click Copilot icon in status bar
- Sign in with GitHub
- Verify in Settings: Extensions → Copilot

**Can't open from terminal**

- Install Shell Command: Cmd+Shift+P → "Shell Command: Install 'code' command in PATH"

---

## Time Check

**Expected: 1-1.5 hours**

If you're significantly over:

- You're being too thorough (good problem!)
- Or hitting technical issues (come ask for help)

If you're significantly under:

- Make sure you actually tried each part
- Did you document your learning?
- Practice the workflow a few more times

---

## What's Next?

**Ho 1: "Git Good"** will build on this foundation:

- Proper project structure for kanyo
- Python virtual environment setup
- Requirements and dependencies
- Documentation framework
- First "real" commit to the project

**But first:** Take a break! You've set up your entire development workflow. That's a real accomplishment.

---

## Questions to Ponder

Before Ho 1, think about:

1. Which Claude Code model did you prefer for what tasks?
2. Are you comfortable with the Git workflow?
3. Any tools/commands you want more practice with?

**If you hit any blockers:** Come back to Claude.ai chat with specific questions.

**When ready for Ho 1:** Return to Claude.ai and say "Ho 0.5 complete, ready for Ho 1" with a brief summary of how it went.

---

**Completed:** \***\*\_\_\_\*\***
**Time Spent:** \***\*\_\_\_\*\***
**Confidence Level (1-5):** \***\*\_\_\_\*\***
**Notes for Next Time:** \***\*\_\_\_\*\***
