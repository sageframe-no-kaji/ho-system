# Infrastructure

Personal practitioner-scope context. The agent reads this so it doesn't have to be retold which GitHub account a project lives in, which machines are reachable, or which deployment pattern applies to which kind of project.

## GitHub accounts

<For practitioners with one account, simplify to a single line. For practitioners with multiple, list each with its purpose.>

- **<account-name-1>**—<purpose, e.g., "development projects, default for code repositories">
- **<account-name-2>**—<purpose, optional>
- **<account-name-3>**—<purpose, optional>

<rules for which account to use for which project type, if applicable>

SSH keys for all accounts are configured in `~/.ssh/config`. <If applicable: don't ask which key to use; the SSH config knows.>

## SSH and machine access

<For practitioners with no remote machines: omit this section or replace with a one-liner. For practitioners with homelab/server access:>

I have SSH access to my machines. Hostnames and key mappings live in `~/.ssh/config`. When operating against a remote, prefer SSH over manual credential entry.

Notable machines (not exhaustive):

- **<hostname-1>**—<purpose, what runs there, deployment pattern>
- **<hostname-2>**—<purpose>
- **<hostname-3>**—<purpose>

For details about specific hosts when working on a project that involves them, the project's CLAUDE.md will specify the host explicitly.

## Vault structure

<Where the practitioner's work lives on disk. Examples:>

Local <organization name> materials live under `~/<vault-root>/`. The structure mirrors the GitHub account split:

- `~/<vault-root>/<account-1>/<project>/`—<what's here>
- `~/<vault-root>/<account-2>/<project>/`—<what's here>

<If the practitioner has the Ho System framework cloned locally, name where:>

The Ho System framework lives at `~/<path-to-framework>/`. The operating discipline that `~/.claude/CLAUDE.md` imports is at `~/<path-to-framework>/practitioner/the-operating-discipline.md`.

## Deployment patterns

The pattern depends on project type. <Adjust this section to match the practitioner's actual deployment patterns. Common patterns below; practitioner may have one, several, or none of these.>

**Production services on self-hosted Docker hosts** (when applicable):

- <skill or tool used for setup, e.g., `sageframe-docker-deploy`>
- File structure: <e.g., `/opt/services/<service-name>/`>
- <other relevant conventions>

**Production services with code-on-host deployment** (when applicable):

- <how this differs from image-based deployment>
- <when this pattern is preferred>

**Public-image deployments** (when applicable):

- <registry pattern, e.g., GitHub Container Registry>
- <use cases>

**Static websites** (when applicable):

- Build target: <e.g., Cloudflare Pages, Vercel, Netlify, GitHub Pages>
- Deploy via: <e.g., Wrangler CLI, vercel CLI, git integration>

**Desktop applications** (when applicable):

- Distribution: <e.g., signed/notarized macOS, Windows installer, AppImage>
- Build via: <e.g., PyInstaller, Tauri>

When initializing a new project, identify which pattern applies and reference the corresponding tooling.

## Conventions

<Practitioner-specific conventions for organizing repos, handling secrets, etc.>

- **Private prompts in public repos**: <e.g., "keep prompt files in `prompts/` and gitignore that directory">
- **Configuration discipline**: `.env.example` is committed; `.env` is gitignored. Never commit secrets.
- **Configuration files**: <project-specific yaml or json templates committed; live versions gitignored>
- **<other conventions, e.g., network locality, git CLI vs. GUI>**

## Reference skills

<Skills the practitioner has installed that other skills or agents might need to invoke. Common pattern:>

- **<skill-name-1>**—<what it does, when to invoke>
- **<skill-name-2>**—<what it does, when to invoke>
- **<skill-name-3>**—<what it does, when to invoke>

<End with brief guidance on when to invoke specific skills, if applicable.>
