# <Practitioner name> — Working Discipline

Auto-loaded by Claude Code at every session. Pulls in the operating discipline (universal Ho System), practitioner profile (yours), and infrastructure context (yours). Language-specific conventions live in `~/.claude/modules/` and are imported by per-project CLAUDE.md files when relevant.

---

## Top-of-mind rules

- Plan first. Restate the ho document, surface ambiguities, propose your approach. Wait for explicit approval before writing code.
- Verification rhythm: LINT. PRODUCE TESTS. EVALUATE TESTS. LINT. COMMIT. Run it after every implementation.
- Coverage floor is 90%. Pre-commit hooks enforce. Don't bypass.
- Type errors get explanatory comments. Silent `# type: ignore` is debt.
- Ask before anything outside the allow list in `settings.json`.

<Practitioners may add 1–2 of their own recurring rules here. Common additions: language-specific gotchas, project-class-specific conventions, repeated mistakes the practitioner wants flagged.>

---

## Operating discipline (Ho System, universal)

@modules/operating-discipline.md

## Practitioner profile

@modules/practitioner.md

## Infrastructure

@modules/infrastructure.md

---

## Language conventions (loaded per-project, not here)

Language-specific conventions live in `~/.claude/modules/`:

- `languages-python.md`—Python projects
- `languages-web.md`—web projects
- `languages-rust.md`—Rust (placeholder; filled in when first Rust project starts)
- `languages-swift.md`—Swift (placeholder; filled in when first Swift project starts)

Per-project `CLAUDE.md` files import the relevant ones. If working in a directory without a project `CLAUDE.md`, ask before applying language-specific conventions.
