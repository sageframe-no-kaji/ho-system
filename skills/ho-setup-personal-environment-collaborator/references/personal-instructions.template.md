# <Practitioner name> — Working Discipline

<Place this in the personal instruction source the agent loads. Replace the module slots below with native imports or explicit instructions to read the resolved files at session start.>

---

## Top-of-mind rules

- Plan first. Restate the ho document, surface ambiguities, propose your approach. Wait for explicit approval before writing code.
- Verification rhythm: LINT. PRODUCE TESTS. EVALUATE TESTS. LINT. COMMIT. Run it after every implementation.
- Coverage floor is 90%. Pre-commit hooks enforce. Don't bypass.
- Type errors get explanatory comments. Silent `# type: ignore` is debt.
- Ask before anything outside the permissions granted in this environment.

<Practitioners may add 1–2 of their own recurring rules here. Common additions: language-specific gotchas, project-class-specific conventions, repeated mistakes the practitioner wants flagged.>

---

## Operating discipline (Ho System, universal)

<load the resolved modules/operating-discipline.md>

## Practitioner profile

<load the resolved modules/practitioner.md>

## Infrastructure

<load the resolved modules/infrastructure.md>

---

## Language conventions (loaded per-project, not here)

Language-specific conventions live alongside the personal modules:

- `languages-python.md`—Python projects
- `languages-web.md`—web projects
- `languages-rust.md`—Rust (placeholder; filled in when first Rust project starts)
- `languages-swift.md`—Swift (placeholder; filled in when first Swift project starts)

Project instructions load the relevant ones. If working in a directory without project instructions, ask before applying language-specific conventions.
