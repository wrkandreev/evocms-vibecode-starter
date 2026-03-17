# Roles And Access

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- Access rules in Evo projects may live in controllers, helpers, manager roles, or custom session checks.
- Frontend behavior can depend on user role, attached resources, or manager permissions.

## Documentation Goals

- identify important roles
- identify where permission checks happen
- identify which data or pages become visible only for certain users

## What Must Be Verified On A Live Project

- exact role ids and meanings
- whether manager permissions are mixed with frontend access rules
- whether access depends on TV values, profile data, or resource relations
