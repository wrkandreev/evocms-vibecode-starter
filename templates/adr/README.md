# ADR Journal

Copy this directory into the project as `core/custom/docs/adr/` during bootstrap. Keep it in the project's main branch so future agent sessions can follow decisions made for the live implementation.

Write an ADR when a decision is expensive to reverse, easy to forget, or likely to be relitigated: choosing a filter mechanism, restructuring a controller, changing data flow between ClientSettings and views, migrating a subsystem, adopting a naming convention.

## File Format

- name files `NNNN-kebab-case-title.md`, numbered sequentially
- keep one decision per file
- add every new ADR to the index table below
- when a decision is replaced, mark the old entry `superseded by NNNN` and keep the file

## Index

| # | Title | Status | Date |
|---|---|---|---|
| 0001 | <decision title> | accepted | <YYYY-MM-DD> |
