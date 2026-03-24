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
.fleet/
*.swp
*.swo
*.orig

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
core/custom/project-sources.local.md

# Composer and npm local artifacts if present
vendor/
node_modules/
core/composer/cache/

# Evo and project runtime cache
assets/cache/*
!assets/cache/.gitignore
core/cache/*
!core/cache/.gitignore
core/storage/cache/*
core/storage/logs/*
core/storage/sessions/*
core/storage/views/*
core/storage/framework/cache/*
core/storage/framework/sessions/*
core/storage/framework/views/*

# Runtime import/export
assets/export/*
assets/import/*

# Uploads only if project policy keeps them outside git
# assets/images/uploads/
# assets/files/uploads/
```

## Important Notes

- Do not ignore actual project source under `core/custom/`, `views/`, `assets/`, `dashboard/`, `ajax/`, or `cron/` by default.
- Ignore only generated runtime data and local machine files.
- Ignore local-only project source registries such as `core/custom/project-sources.local.md`, but keep an example template in git if needed.
- Do not ignore deployable manager side config files such as `assets/modules/clientsettings/config/*.php`, `assets/plugins/templatesedit/configs/*.php`, `assets/tvs/multitv/configs/*.php`, `assets/plugins/pagebuilder/config/*`, selector field controller/config directories such as `assets/tvs/selector/lib/*`, or package config directories such as `core/custom/directory/*`.
- These config files should stay in git so that `git pull` on production delivers manager changes to the right place.
- If the project uses PageBuilder, treat `assets/plugins/pagebuilder/config/` as deployable project code and keep the whole config directory in git.
- If the project uses Selector, treat project selector controller/config files under `assets/tvs/selector/lib/` as deployable project code and keep that directory in git.
- If the project uses `evocms-directory`, treat `core/custom/directory/` as deployable package config and keep that directory in git.
- If the project adds custom `DocLister` controllers, config files, or DL filters, keep those project additions in git.
- Do not treat package asset files such as `assets/modules/directory/css/style.css` as project config by default; keep only project owned overrides or published config that actually belongs to the repository workflow.
- Do not treat `assets/lib/` as required project source by default.
- In typical Evo projects, `assets/lib/` comes from packages such as `DocLister` and is not the place for project specific edits.
- Do not pull `assets/lib/` into project git just because it exists on disk.
- Prefer keeping project owned `DocLister` extension points such as controllers, configs, and custom filters instead of bundled package assets under `assets/lib/`.
- If active views load CSS, JS, images, fonts, or build output from a separate directory such as `html/` or a custom frontend folder, treat that directory as active project source and keep it in git.
- Upload directories depend on project policy. Some teams version seed media, others keep all user uploads out of git.
- If a project already has a server side exclude policy, document it separately and do not silently mirror it into repository ignore rules.

## Official Package Reference

- ClientSettings official repository: `https://github.com/evocms-community/clientsettings`
- Directory official repository: `https://github.com/evocms-community/evocms-directory`
- MultiTV official repository: `https://github.com/extras-evolution/multiTV`
- PageBuilder official repository: `https://github.com/evocms-community/pagebuilder/tree/master`
- Selector official repository: `https://github.com/Pathologic/Selector/tree/master`
- DocLister official repository: `https://github.com/AgelxNash/DocLister`
- If local examples are missing and the task touches ClientSettings config, prefer that repository over open internet search.
- If local examples are missing and the task touches Directory config, prefer that repository over open internet search.
- If local examples are missing and the task touches MultiTV config, prefer that repository over open internet search.
- If local examples are missing and the task touches PageBuilder config, prefer that repository over open internet search.
- If local examples are missing and the task touches Selector config or controller files, prefer that repository over open internet search.
- If local examples are missing and the task touches `DocLister` controllers, configs, or filters, prefer that repository over open internet search.

## What Must Be Verified On A Live Project

- which cache directories are actually written at runtime
- whether `core/storage/` contains only runtime files or also project-owned code in that specific installation
- whether a specific project adds custom `DocLister` controllers, configs, or filters that should be versioned
- whether active views depend on a separate frontend asset directory such as `html/`
- whether `vendor/` is committed or installed during deploy
- whether uploads belong in git for that project
- whether extra secret files exist besides `core/custom/define.php`
