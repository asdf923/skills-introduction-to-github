# AGENTS.md

## Cursor Cloud specific instructions

This repository is a **GitHub Skills tutorial** ("Introduction to GitHub") — not a traditional application. It contains only Markdown content, images, and GitHub Actions workflows. There is no application code, no package manager, no build system, and no runtime.

### Repository architecture

- `.github/workflows/` — 5 GitHub Actions YAML workflows forming a step-based state machine (steps 0–4)
- `.github/steps/` — Markdown content for each tutorial step; `-step.txt` tracks current step number
- `images/` — PNG screenshots used in the tutorial
- `README.md` — rendered tutorial content (auto-updated by workflows via `skills/action-update-step@v2`)

### Linting

Since this is a content + workflow repo, the relevant linting tools are:

| Tool | Command | What it checks |
|------|---------|----------------|
| `actionlint` | `actionlint` | GitHub Actions workflow syntax and semantics |
| `yamllint` | `yamllint -d relaxed .github/workflows/ .github/dependabot.yml` | YAML syntax (use `-d relaxed` to avoid false positives on line-length in upstream template) |
| `markdownlint` | `markdownlint --disable MD013 MD033 MD041 -- '**/*.md'` | Markdown structure (disable MD013/MD033/MD041 as the template uses inline HTML and long lines by design) |

### Important notes

- **No services to start.** All tutorial logic runs on GitHub.com via GitHub Actions. There is no local server, database, or backend.
- **yamllint PATH**: The `yamllint` binary installs to `~/.local/bin`. If not on PATH, run `export PATH="$HOME/.local/bin:$PATH"` first.
- **Pre-existing style warnings**: The upstream GitHub Skills template has intentional line-length violations and inline HTML usage. These are not bugs.
- The workflow state machine tracks progress in `.github/steps/-step.txt` (a single integer). Each workflow reads this file to decide if it should run.
