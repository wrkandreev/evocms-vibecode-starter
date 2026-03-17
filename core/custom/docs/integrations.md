# Integrations

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- Evo projects often include external integrations beyond page rendering.
- Common examples are bots, webhooks, email services, third party APIs, and custom business process endpoints.

## Documentation Goals

- list integration entry points
- identify handler classes and routes
- record side effects such as cache clears, resource creation, or notifications
- flag secret bearing files and values

## What Must Be Verified On A Live Project

- active integrations
- routes and webhooks
- storage of secrets and tokens
- which roles or templates are affected by integrations
