## The Operating Discipline (framework scope)

The philosophical/operational document we drafted today. It sits in `practitioner/operating-discipline.md` in the Ho System repo. Its job: state what discipline the practice runs under, and why.

What it contains: principles. Practice-not-productivity. Understanding over compliance. The verification stack as a four-layer concept (lint → type-check → test → optional cross-model). Permissions discipline. Real-time monitoring. Planning mode. Bounded sessions. Black box calibration. The encoded environment principle.

It does _not_ contain configuration files. It says "linting runs before commit, the commit does not happen if linting fails." It does _not_ say "here is your `.pre-commit-config.yaml`." Configuration is downstream.

The audience: the practitioner reading to understand the practice. Also: the environment setup skill, which uses the discipline as source material.

## The Environment Setup Skill (`ho-setup-personal-environment-collaborator`)

The skill that turns discipline into encoded environment. Practitioner scope — runs once per practitioner-tool combination, not per project.

What it produces: actual configuration files that live in the practitioner's tools and travel across projects. CLAUDE.md (or codex-config or VS Code settings depending on the IDE), pre-commit hook templates, baseline `pyproject.toml` with ruff/mypy/pytest configured to the discipline's standards, `.gitignore` defaults, opinionated folder layout templates.

How it works: reads the operating discipline, interrogates the practitioner about their specific stack (language, IDE, model access, preferred tooling), generates configurations that encode the discipline for that practitioner's specific environment. The skill is opinionated about defaults but adapts to the practitioner's specific tools.

The relationship to the discipline document is direct: the discipline says _what_ must be true; the skill produces files that _make it true_ for this practitioner. Where the discipline says "above 90% test coverage is the target," the skill produces a `pyproject.toml` with `--cov-fail-under=90` configured. Where the discipline says "linting runs before commit," the skill produces a `.pre-commit-config.yaml` with ruff in the hooks list.

## Per-Ho Documents (Kamae 5, project scope)

The bounded scope for a single session. Project scope — one document per ho, written at session start.

What it contains: what THIS specific ho does. The ho's narrative, dependencies, in-scope, out-of-scope, what "done" means _for this ho_, decisions to resolve, verification gates _specific to this work_.

What it doesn't contain: the universal discipline. The ho doesn't say "remember to lint before commit" — that's already encoded in the environment, the agent knows it, the pre-commit hooks enforce it. The ho doesn't say "use type hints" — same.

What the ho _does_ say about testing/verification is specific to its work: "tests cover the parser with synthetic fixture, parser with malformed input, parser with empty export." The ho specifies _what behaviors_ must be tested for its deliverable. The discipline is universal; the ho is per-session.

## How Testing/Verification Flows Through the Three Layers

This is where the encoded environment principle pays off. The discipline names the standard once. The environment encodes it once. The ho specifies the work that meets it for this specific session. Each layer says something different and they don't repeat each other.

**Operating discipline (universal):**

> "Testing is non-negotiable. Tests are specifications, not QA. Coverage target is above 90%. The lint-test cycle runs before every commit."

**Environment setup output (per practitioner):**

- `pyproject.toml` with `[tool.pytest.ini_options]` configured for src/ layout and coverage reporting
- `.pre-commit-config.yaml` running ruff, mypy strict, and pytest with coverage threshold
- CLAUDE.md telling the agent: "Always run `pytest --cov` before committing. The discipline requires all gates green."

**Per-ho document (per session):**

> "Tests cover: the parser with a synthetic 5-conversation fixture, the parser with a malformed JSON file, the parser with an empty export. Manual inspection: spot-check 20 thread groupings on real data. Done means lint clean, mypy strict-clean, pytest at >=90% coverage with all new tests passing."

The ho's testing language is specific to what it's testing. It doesn't restate the universal standard because the universal standard is already true wherever the agent is working — the environment enforces it.

## The Practical Implication

A new practitioner who has run the environment setup skill once doesn't have to be reminded about lint and test discipline in every ho. The reminders would be noise; the discipline is already encoded in their tools. What they need from each ho document is what's _specific to that session_ — what's being built, what behaviors to test, what acceptance criteria are.

A practitioner who has _not_ run the environment setup is in a different situation. The discipline isn't encoded yet. They're holding it in their head, manually running lint and tests, manually enforcing the standards. That works for an experienced practitioner who has internalized the discipline (you, currently — Three Hours described exactly this), but it's brittle. A fresh agent dropping into the project doesn't have the encoded reminders. The environment-setup skill exists to remove that brittleness.

This is also why the environment-setup skill matters more than it initially seems. Without it, the discipline lives in the practitioner's head and the agent has to be told it every session. With it, the discipline lives in the project's artifacts and the agent reads it the way it reads code. Same shift the Three Hours essay described — agent conformance to encoded standards rather than instruction-by-instruction reminders.
