# Troubleshooting

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Common Situations

### A TV exists in code assumptions but not in real project

- verify the field was actually created in CMS
- verify registry is fresh
- verify the TV is bound to the expected template

### A TV exists but editors do not see it in manager

- verify `templatesedit` config
- verify template binding and manager grouping

### MultiTV data shape is unclear

- verify dedicated MultiTV config
- verify field captions and stored keys on the live project

### Selected resources look wrong

- verify selector controller mapping
- verify selector filters and allowed template ids

### View or controller mapping looks inconsistent

- verify registry output if installed
- verify template level controller configuration in database
- verify whether fallback conventions are in use

### Code changed but output did not

- verify cache layers
- verify generated artifacts are not stale
- verify production and local environments are aligned
