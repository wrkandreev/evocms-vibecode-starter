# Gitignore

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Goal

- Track the whole project source.
- Ignore operating system files, editor files, caches, logs, and local secret files.
- Keep deployable project code in git without dragging in machine specific noise.

## Recommended Baseline

Use a project root `.gitignore` close to this:

```gitignore
# OS files
.DS_Store
Thumbs.db
Desktop.ini

# Editors and IDEs
.idea/
.vscode/
*.swp
*.swo

# Logs and temp
*.log
*.tmp
*.bak

# Environment and local secrets
.env
.env.*
!.env.example
core/custom/define.php
!core/custom/define.php.example

# Composer and npm local artifacts if present
vendor/
node_modules/

# Evo and project runtime cache
assets/cache/*
!assets/cache/.gitignore
core/cache/*
!core/cache/.gitignore

# Session and generated runtime files
assets/export/*
assets/import/*

# Uploads only if project policy keeps them outside git
# assets/images/uploads/
# assets/files/uploads/
```

## Important Notes

- Do not ignore actual project source under `core/custom/`, `views/`, `assets/`, `dashboard/`, `ajax/`, or `cron/` by default.
- Ignore only generated runtime data and local machine files.
- Do not ignore deployable manager side config files such as `assets/modules/clientsettings/config/*.php`, `assets/plugins/templatesedit/configs/*.php`, or `assets/tvs/multitv/configs/*.php`.
- These config files should stay in git so that `git pull` on production delivers manager changes to the right place.
- Upload directories depend on project policy. Some teams version seed media, others keep all user uploads out of git.
- If a project already has a server side exclude policy, document it separately and do not silently mirror it into repository ignore rules.

## What Must Be Verified On A Live Project

- which cache directories are actually written at runtime
- whether `vendor/` is committed or installed during deploy
- whether uploads belong in git for that project
- whether extra secret files exist besides `core/custom/define.php`
