# Cron

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- Some Evo projects contain background jobs outside the normal page request flow.
- Cron logic may live in root scripts, `cron/`, package classes, or custom controllers.

## Documentation Goals

- identify cron entry points
- identify task controller files
- document command format and scheduling assumptions
- list side effects such as updates, unpublishing, notifications, cache clears

## What Must Be Verified On A Live Project

- actual cron entry commands
- task dependencies and environment assumptions
- whether tasks are safe to run manually
