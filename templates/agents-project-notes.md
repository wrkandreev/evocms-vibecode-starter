# AGENTS Project Notes Template

Append this section to the project `AGENTS.md` during bootstrap and fill it with confirmed facts only. Update it whenever work verifies new facts. Never record tokens, passwords, private keys, or other real secret values here.

Division of labor: the project README records the stable project card; this section records working facts and gotchas that change during development.

Remove subsections that do not apply. Add new verified facts under Component Facts or Gotchas as work proceeds.

---

## Project Notes: <project-domain>

This section records facts verified for this repository and host. Prefer these facts over rediscovering them in future sessions.

### Infrastructure

- production SSH: `<user@host>`
- production web root: `<absolute path>`
- Git remote: `<remote url>`
- production branch: `<branch>`
- deploy entry point: `<signed Gitea webhook POST /deploy, token protected deploy.php, or manual git pull>`

### Local Environment

- local workspace path: `<path>`
- local PHP or tooling quirks: `<for example php.exe not in PATH, elevated sandbox permissions>`
- Git or SSH client quirks: `<for example core.sshCommand path mangling, quoting workarounds>`

### Active Architecture

- active project code: `<core/custom packages, views, frontend directories, ajax, cron>`
- controller style: `<TemplateController::process(), legacy constructor style, mixed>`
- installed and actually used components: `<ClientSettings, templatesedit, MultiTV, Selector, DocLister, payment modules>`
- template registry: `<installed / not installed; where generated context lives; read and write API policy>`

### Secrets And Forbidden Files

- real secrets: `<location on production>`
- committed templates: `<.example files>`
- never track: `<verified forbidden paths>`

### Validation

- `<commands run before handoff, for example php -l on changed files, git diff --check>`

### Component Facts

- `<verified component-specific facts, for example news catalog ids, exact TV names, filter behavior, ClientSettings keys that code depends on>`

### Gotchas

- `<hosting quirks, cache behavior, view naming exceptions, anything that already caused one wrong conclusion>`
