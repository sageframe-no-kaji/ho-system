# Languages: Web

Conventions for web work—HTML, CSS, JavaScript, and the static-site or app-frontend tooling around them. Two sections: universal Ho System web conventions (light) and my specifics (overlay).

This module is imported by project-level CLAUDE.md files when the project includes web work. It is not loaded globally.

---

## Universal Ho System web conventions

Ho is opinionated about Python because Python is where the framework's verification stack is most fully encoded. Web work has more variation by project type, so the universal conventions are lighter.

### Prefer the simplest tool that does the job

The framework is wary of build chains that solve problems the project doesn't have. A static site of 20 pages doesn't need a SPA framework. A page with a single dynamic widget doesn't need React. Reach for the minimal tool first; add complexity when there's a concrete reason.

### Lint and format

Use the ecosystem standard for the toolchain in play. Biome (newer, single-tool) or ESLint + Prettier (older, two-tool). Don't mix conventions in a single repo.

Format on save in the editor. Lint on commit via pre-commit or the toolchain's equivalent.

### Package management

`package.json` for projects that need npm. Lockfile committed (`package-lock.json`, `pnpm-lock.yaml`, or equivalent). `node_modules/` gitignored.

### Configuration discipline

- Secrets in `.env`, gitignored. `.env.example` committed.
- Build artifacts (`dist/`, `build/`, `_site/`, `.next/`, `.nuxt/`) gitignored.

---

## My specifics (Andrew's overlay)

Two kinds of web work, distinct stacks.

### Static sites: Eleventy + Cloudflare Pages

For content-driven sites (atmarcus.net pattern):

- **Eleventy** as the static site generator. Minimal config, file-based routing, Nunjucks templates.
- **Plain HTML, CSS, JS** in the rendered output. No bundler, no transpiler, no build step beyond Eleventy itself.
- **Cloudflare Pages** as the deploy target. Either Wrangler CLI (`npx wrangler pages deploy public`) or Cloudflare Pages git integration.
- **package.json** scripts for build (`eleventy`) and dev server (`eleventy --serve`).
- Repo lives under the `sageframe-dharma` GitHub account.

Why this stack: minimum machinery between content and deployed site. Single mental model.

### App frontends: HTMX + Jinja2 + Tailwind + Alpine

For frontends served by a Python backend (Glassroom pattern, Shodō pattern):

- **HTMX** for server-driven interactivity. Forms post to FastAPI endpoints; HTMX swaps response fragments into the page. No SPA framework.
- **Jinja2** for server-side templates rendered by FastAPI.
- **Tailwind CSS** for styling. Utility-first; configured per project; build step via Tailwind CLI or PostCSS.
- **Alpine.js** for sprinkled client-side interactivity (toggles, dropdowns, form behavior that doesn't need a server round-trip).
- **D3 or Plotly islands** when a specific page needs interactive data visualization.

The principle: the page is HTML rendered on the server; the client gets light interactivity layered on top. No build pipeline shipping a JavaScript application.

### Deployment patterns

- Static sites → Cloudflare Pages (above).
- App frontends → deployed alongside their Python backend, typically via `sageframe-docker-deploy` for self-hosted or via Cloudflare Pages + Workers for serverless.

Deployment infrastructure detail in `@modules/infrastructure.md`.

### File organization

Static sites (Eleventy):

```
project-root/
├── src/                      # source content (markdown, templates, assets)
├── _site/                    # build output (gitignored)
├── package.json
├── .eleventy.js
└── wrangler.toml             # Cloudflare config
```

App frontends (served by Python):

- HTML templates: `src/<package>/templates/`
- CSS: `src/<package>/static/css/` (Tailwind input + compiled output)
- JS: `src/<package>/static/js/` (Alpine, HTMX vendored or via CDN)
- Build script: `scripts/build_frontend.sh` or equivalent

---

## Templates

Concrete starting files for new web projects live in `~/.claude/templates/`:

- `gitignore.web.baseline`—`node_modules/`, `dist/`, `build/`, `.env`, etc.

No `package.json.baseline` because dependencies vary too much by project type. The skill (when built) will interrogate at project creation.
