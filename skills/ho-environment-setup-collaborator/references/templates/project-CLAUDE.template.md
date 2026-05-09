# <project-name>

<one-line description of what this project IS and what it does>

## Languages

Uncomment the module imports that match this project's stack:

@~/.claude/modules/languages-python.md
<!-- @~/.claude/modules/languages-web.md -->
<!-- @~/.claude/modules/languages-rust.md -->
<!-- @~/.claude/modules/languages-swift.md -->

## Project-specific rules

<list of rules that apply only to this project. Examples:>

- Full spec: see `prompts/<spec-file>.md`
- Lint with ruff before completing any ho
- <project-specific gotcha>
- <project-specific convention>
- Secrets in `.env` only

## Project-specific conventions

<optional section for libraries or patterns that have non-obvious usage requirements. Examples from existing projects:>

<!-- ## Playwright (glassroom example)
- Never use wait_until='networkidle'; pages with long polling never settle
- Use wait_until='domcontentloaded' and explicit waits for known elements -->

<!-- ## FastAPI + HTMX (kanyō-viewer example)
- Server-rendered fragments; no SPA framework
- HTMX endpoints return HTML partials, not JSON -->

## References

<links to project-specific external docs, related repos, or framework references>

- Project repo: <repo URL>
- Related: <other relevant repos or docs>
