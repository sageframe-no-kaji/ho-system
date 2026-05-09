~/.claude/
│
├── CLAUDE.md [MASTER, ~40-60 lines]
├── settings.json [PERMISSIONS, ~40-60 lines]
│
├── modules/
│ ├── operating-discipline.md [UNIVERSAL HO, ~150-200 lines]
│ ├── practitioner.md [PERSONAL: Andrew, ~30-50 lines]
│ ├── languages-python.md [HO + PERSONAL, ~150-250 lines]
│ ├── languages-web.md [HO + PERSONAL, ~80-150 lines]
│ ├── infrastructure.md [PERSONAL: Andrew, ~80-150 lines]
│ ├── languages-rust.md [STUB, ~10 lines]
│ └── languages-swift.md [STUB, ~10 lines]
│
├── templates/
│ ├── pyproject.baseline.toml [UNIVERSAL HO, ~100-150 lines]
│ ├── pre-commit.baseline.yaml [UNIVERSAL HO, ~30-50 lines]
│ ├── gitignore.baseline [HO + PERSONAL, Python flavor, ~50-80 lines]
│ ├── gitignore.web.baseline [UNIVERSAL HO, web flavor, ~30-50 lines]
│ ├── env.example.baseline [UNIVERSAL HO, ~20-30 lines]
│ └── project-CLAUDE.template.md [UNIVERSAL HO, ~20-40 lines]
│
└── skills/ [EXISTING + ADDITIONS]
├── sageframe-cartographer.skill [already there]
├── sageframe-config-sync.skill [already there]
├── sageframe-docker-deploy.skill [already there]
└── di.skill [add: copy from your existing skills location]

What each file is
Top-level
CLAUDE.md — Loaded by Claude Code every session, in every directory under your home folder that doesn't have its own project-level CLAUDE.md (and even then, this loads first as foundation). Contains: a brief identity header (you, your role), three or four core operational rules visible at the top so they aren't buried, then @imports to every module file. The substance lives in the modules; this file is the spine that pulls them all in.
settings.json — Claude Code reads this for permissions, theme, model defaults, hooks. Tightened-with-overrides per our earlier discussion. Specific allow patterns for high-frequency safe operations (test runners, linters, read-only git, Python execution, file ops in your Vaults). Specific deny patterns for destructive/network/secret-touching operations. skipDangerousModePermissionPrompt: false re-enabled.
Modules
operating-discipline.md — The Ho System universal operating discipline. Same content for any Ho practitioner. For tonight: a copy of (or symlink to) practitioner/operating-discipline.md from your framework repo. The skill holds a canonical copy in its references/ and places it here when run for any practitioner.
practitioner.md — Andrew-specific. Who you are, your background (architect, MIT M.Arch, started writing software late 2025, Ho System creator), the kind of practitioner you are (systems thinker, not a debugger; you read code carefully and make architectural decisions; you've shipped Kanyō to production). What "communicate with me" looks like at the technical-collaboration level: direct, push back when wrong, surface uncertainty, no hedging on recommendations. NOT prose voice rules — those are chat territory.
languages-python.md — The substantive one. Two sections, clearly marked:

Universal Ho System Python conventions: ruff for lint+format (replacing flake8+black), mypy in strict mode, pytest with --cov-fail-under=90, src/ layout, .env.example committed with structure, type error discipline applied to Python (the three-categories approach), # type: ignore always paired with comment.
Andrew's Python specifics: uv for package management (not pip+venv+pip-tools), line length 100, Python 3.11+ (TBD final number), specific test discovery patterns, the Logger event() override pattern noted as a known exception, your conventions for /Agent-Tasks/ and prompts/ private patterns.

languages-web.md — Smaller and more configurable than Python. Two sections:

Universal Ho System web conventions: honestly minimal — Ho doesn't have strong opinions about React vs HTMX vs Vue. Maybe: prefer the simplest tool that does the job; static sites prefer no build step when possible; lint with whatever the ecosystem agreed on (Biome or ESLint+Prettier).
Andrew's web specifics: Eleventy + Nunjucks + Cloudflare Pages for static sites (atmarcus.net pattern); HTMX + Jinja2 + Tailwind + Alpine.js for app frontends with Python backends (kanyō-viewer / shodō pattern); package.json conventions; deploy via Wrangler CLI or Cloudflare Pages dashboard.

infrastructure.md — Fully personal. Your three GitHub accounts and what each is for (sageframe-dharma for websites, sageframe-iroro for Obsidian/configs, sageframe-no-kaji for development). Pointer to ~/.ssh/config (don't duplicate it; just tell the agent it exists and what general topology it represents). SSH-accessible machines (kanyō deployment target, mandala for Baserow, your other homelab nodes — not exhaustive, but the named ones the agent might encounter). Deployment patterns by project type. Reference to sageframe-docker-deploy skill as the canonical Docker conventions.
languages-rust.md, languages-swift.md — Stubs. Each is a header plus one paragraph: "Filled in when first project happens. Skill will interrogate at that point." Wired into CLAUDE.md so the @import doesn't fail; content gets fleshed out later.
Templates
pyproject.baseline.toml — Concrete file, ready to drop into a new Python project. ruff config with your line length, target Python version, selected rule set; mypy strict config; pytest config with --cov-fail-under=90; [project] metadata with placeholders for name/description/version; [dependency-groups.dev] for uv-managed dev deps; build-system block (uv-compatible).
pre-commit.baseline.yaml — Concrete pre-commit config. ruff format, ruff check, mypy, optional pytest (pytest pre-commit can be slow on larger suites; we'll discuss).
gitignore.baseline (Python flavor) — Python defaults plus your conventions: .env, .venv, **pycache**, _.pyc, .coverage, htmlcov/, .mypy_cache, .pytest_cache, .ruff_cache, prompts/, /Agent-Tasks/, .obsidian/, .DS_Store. The kanyō-style media exclusions (_.mp4, clips/, etc.) are project-specific and stay out of the baseline.
gitignore.web.baseline — node_modules/, dist/, build/, .next/, .nuxt/, \_site/, .cache/, .env, .DS_Store. Plus the prompts privacy pattern.
env.example.baseline — Pattern showing your .env.example structure: section dividers (──────), comments explaining each section, REQUIRED markers, no actual secret values.
project-CLAUDE.template.md — Based on glassroom's shape. Project name, one-line what-it-is, project-specific rules section (with placeholders for the practitioner to fill in: lint commands, project-specific gotchas, references to project's spec), project-specific section for any framework or library quirks. Short by design — does NOT restate the universal discipline because the global CLAUDE.md already loaded that.
