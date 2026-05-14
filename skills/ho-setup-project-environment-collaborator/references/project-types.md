# Project Types

The Ho System recognizes five project types. The type drives scaffolding decisions—language modules to import, source layout, verification stack, deployment pattern, distribution method.

## Service

A long-running process exposed over a network or driven by a schedule. Examples: API backends, scheduled jobs, observability collectors, message processors, computer-vision pipelines.

**Language:** typically Python; may be Rust for performance-critical services.

**Source layout:**
```
src/[package]/
├── __init__.py
├── main.py                   # entry point
├── api/                      # HTTP handlers
├── domain/                   # business logic
├── infra/                    # external integrations
└── utils/                    # shared utilities
tests/
docs/
configs/                      # configuration templates
```

**Verification stack:** ruff + mypy + pytest with ≥90% coverage.

**Deployment:** Docker via the practitioner's docker-deploy pattern (e.g., `sageframe-docker-deploy`). Includes `Dockerfile`, `docker-compose.yml`, and operational scripts in `scripts/`. Configuration via `.env` and `configs/config.yaml`.

**Distribution:** not applicable (services are deployed, not distributed).

**CLAUDE.md imports:** `@~/.claude/modules/languages-python.md` (or rust); plus the practitioner's infrastructure module is already loaded globally.

## Desktop App

A user-facing application installed on the practitioner's or end-user's machine. Examples: media converters, knowledge-management tools, writing environments.

**Language:** Python with PyInstaller for cross-platform; Swift for macOS-native; Tauri/Rust for cross-platform with web frontend.

**Source layout (Python case):**
```
src/[package]/
├── __init__.py
├── main.py                   # entry point (becomes the executable)
├── ui/                       # interface code (Tk, PyQt, etc.)
└── core/                     # application logic
tests/
docs/
man/                          # CLI man pages if app has a CLI
installer/                    # platform-specific installer configs
└── windows/                  # Inno Setup
└── macos/                    # notarization scripts
build.spec                    # PyInstaller spec
```

**Verification stack:** ruff + mypy + pytest with ≥90% coverage.

**Distribution:** signed/notarized macOS `.app`, Windows installer (Inno Setup), Linux AppImage if applicable. Released via GitHub Releases.

**CLAUDE.md imports:** `@~/.claude/modules/languages-python.md` (or swift); often plus a Dockerfile reference if the app has a server component.

## Library or CLI

A reusable component or command-line tool meant to be installed by other projects or end-users. Examples: pure Python utilities, CLI wrappers, framework extensions.

**Language:** typically Python; sometimes Rust for performance libraries.

**Source layout:**
```
src/[package]/
├── __init__.py
├── core.py                   # primary public surface
└── cli.py                    # CLI entry point if applicable
tests/
docs/
```

**Verification stack:** ruff + mypy strict + pytest with ≥90% coverage. Libraries have higher coverage standards than services because they're consumed by other code that depends on stable behavior.

**Distribution:** PyPI when public; GitHub Releases when private. The `[project.scripts]` block in `pyproject.toml` defines CLI entry points.

**CLAUDE.md imports:** `@~/.claude/modules/languages-python.md`.

## Static Site

A content-driven website with no application logic on the server. Examples: blogs, documentation sites, portfolios, marketing pages.

**Language:** web (HTML, CSS, JS), with a static site generator (Eleventy, Astro, Hugo, etc.).

**Source layout (Eleventy case):**
```
src/                          # source content (markdown, templates)
├── _includes/                # layouts and partials
├── _data/                    # site-wide data
└── posts/                    # content
.eleventy.js                  # config
package.json
wrangler.toml                 # if Cloudflare Pages
public/                       # static assets (gitignored if generated; committed if hand-managed)
_site/                        # build output (gitignored)
```

**Verification stack:** the toolchain's defaults (e.g., Eleventy's lint, prettier formatter). No coverage measurement.

**Deployment:** Cloudflare Pages, Vercel, Netlify, or GitHub Pages depending on the practitioner's preference.

**Distribution:** not applicable (sites are deployed).

**CLAUDE.md imports:** `@~/.claude/modules/languages-web.md`.

## App Frontend

A web application served by a backend the practitioner also owns. Combines server-rendered HTML with sprinkled client-side interactivity. Examples: internal tools, dashboards, viewers for production data.

**Language:** Python (backend) + web (frontend).

**Source layout:**
```
src/[package]/
├── __init__.py
├── main.py                   # FastAPI app
├── api/                      # JSON endpoints (when applicable)
├── views/                    # HTMX endpoints returning HTML fragments
├── templates/                # Jinja2 templates
└── static/
    ├── css/                  # Tailwind input + compiled output
    └── js/                   # Alpine, HTMX
tests/
docs/
package.json                  # for Tailwind build
tailwind.config.js
```

**Verification stack:** ruff + mypy + pytest for the Python side; toolchain defaults for the web side.

**Deployment:** typically alongside the Python backend—Docker via docker-deploy pattern, or Cloudflare Workers if the backend is small enough.

**CLAUDE.md imports:** `@~/.claude/modules/languages-python.md` AND `@~/.claude/modules/languages-web.md`.

## Full-stack JS/TS App

A web application where a JavaScript/TypeScript framework handles both frontend rendering and server-side logic — no separate Python backend. Examples: SvelteKit apps, Next.js apps, Remix apps. Use when the domain calls for a single-language stack or when the frontend complexity warrants a full JS framework.

**Language:** TypeScript throughout.

**Source layout (SvelteKit case):**
```
src/
├── app.html              # SvelteKit HTML shell
├── app.css               # Tailwind v4 entry point (@import "tailwindcss")
├── app.d.ts              # SvelteKit ambient types
├── lib/                  # shared utilities and server-only code
│   ├── db.ts             # database access
│   └── index.ts          # public lib barrel
└── routes/               # file-based routing
    ├── +layout.svelte    # root layout (imports app.css)
    └── +page.svelte      # routes...
static/                   # static assets
tests/                    # Vitest unit tests
```

**Verification stack:** Biome (lint + format for `.ts`/`.js`/`.svelte`) · `svelte-check` (TypeScript strict across Svelte component internals) · Vitest with **80% coverage floor** (component tests are harder to drive to 90% than pure TS).

**Pre-commit:** lefthook running biome → vitest → svelte-check on every commit.

**Key packages:**
- `@sveltejs/adapter-node` — Node/Docker deployment; `adapter-cloudflare` or `adapter-vercel` for serverless
- `tailwindcss` + `@tailwindcss/vite` — Tailwind v4, no separate `tailwind.config.js`
- `better-sqlite3` — SQLite for homelab projects; add `@types/better-sqlite3`
- `@biomejs/biome`, `svelte-check`, `vitest`, `@vitest/coverage-v8`, `lefthook` — all devDependencies

**Deployment:** `adapter-node` + Docker for homelab. Use `sageframe-docker-deploy` for the full deployment pass.

**Distribution:** not applicable (apps are deployed).

**CLAUDE.md imports:** `@~/.claude/modules/languages-web.md`.

**Reference project:** `edelmore-diary` (SvelteKit 2 + Svelte 5 + SQLite + Tailwind v4 + Docker homelab).

## Type-Independent Decisions

Some scaffolding decisions don't depend on project type:

**`.gitignore`:** Python flavor (`gitignore.baseline`) for any Python project, including app frontends. Web flavor (`gitignore.web.baseline`) for static sites. Both for app frontends.

**`.env.example`:** present in any project that reads environment variables (services, app frontends, CLIs that take API keys, static sites with build-time secrets).

**`README.md`:** present in every project. Stub created by the skill; expanded by the practitioner.

**Pre-commit hooks:** present in every Python project. Optional for static sites depending on the toolchain.

**Coverage floor (90%):** applies to every Python project. Static sites don't have one.
