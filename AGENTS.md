# AGENTS.md

## Structure

- Keep maintenance workflows under `.github/workflows/`.
- Keep reviewed workflow configuration under `config/`.
- Prefer direct workflow steps over repository-specific helpers.

## Style

- Sort unordered peer entries by value shape: simple or single-line values first,
  then structured or multiline values, alphabetically within each group.
- Sort unordered peer headings, lists, and table rows alphabetically. Preserve
  narrative, procedural, dependency, interface, priority, and chronological order.
- Sort mise tools, tasks, workflow structure, Renovate rules, and Prek hooks
  consistently with the other homelab repositories.
- Use `.yaml`, never `.yml`.

## Verification

- Run `mise run check` before handoff.
