# Deploy

> Important: deploy workflow is not a standard Evolution CMS CE feature.
> Browser triggered deploy endpoints are a custom project level pattern used for vibe-coding convenience.
> Before applying it, verify the real implementation and security model on the live project.

## Confirmed Pattern

- Evo projects may use shell scripts or direct git based deploys.
- Some projects may additionally use browser triggered deploy endpoints as a custom workflow.
- Deploy details often depend on server SSH context, keys, known hosts, and tokens.

## Important Clarification

- Browser deploy is not an Evo standard.
- It is a custom workflow pattern introduced to let a project owner or coding agent trigger deploy without opening a manual SSH session for every update.
- Treat it as project specific infrastructure, not as a built in CMS capability.

## Documentation Goals

- describe deploy entry points
- record required environment variables or constants
- note branch and remote assumptions
- document lock files, error codes, and safety rules

## Recommended Secret Location

- Prefer storing deploy related secrets and local constants in `core/custom/define.php`.
- Commit `core/custom/define.php.example`, not the real file.
- If `.env` is also supported, document precedence clearly.

## Safety Rules

- do not expose real tokens or keys in repository docs
- do not run production SSH commands without explicit user request
- verify whether deploy is fast forward only or allows merge pulls

## What Must Be Verified On A Live Project

- actual deploy script path
- secret loading rules
- SSH key and known hosts requirements
- whether browser deploy runs under a different user context
- whether this project uses browser deploy at all or only shell or CI based deploy
