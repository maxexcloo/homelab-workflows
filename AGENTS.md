# AGENTS.md

## Structure

- Keep maintenance workflows under `.github/workflows/`.
- Keep reviewed workflow configuration under `config/`.
- Prefer direct workflow steps over repository-specific helpers.

## Style

- Sort mise tools, tasks, workflow structure, Renovate rules, and Prek hooks consistently with the other homelab repositories.
- Use `.yaml`, never `.yml`.

## Verification

- Run `mise run check` before handoff.
