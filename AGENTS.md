# AGENTS.md

## Structure

- Keep maintenance workflows under `.github/workflows/`.
- Keep only `AGENTS.md` and `README.md` as root Markdown files; put other project
  documentation in `docs/`.
- Keep reviewed workflow configuration under `config/`.
- Prefer direct workflow steps over repository-specific helpers.

## Style

- Sort unordered peer entries by value shape: simple or single-line values first,
  then structured or multiline values, alphabetically within each group.
- Sort unordered peer headings, lists, and table rows alphabetically. Preserve
  narrative, procedural, dependency, interface, priority, and chronological order.
- Sort mise tools, tasks, workflow structure, Renovate rules, and Prek hooks
  consistently with the other homelab repositories.
- Use `.yaml`, never `.yml`, for project-owned YAML files unless external tooling
  requires a fixed filename.
- Preserve `LICENSE` and its legal text; never relicense without explicit approval.
- Use Australian English throughout authored prose and every project-owned name,
  including identifiers, configuration keys, environment variables, paths, CLI
  commands, and options. Update every producer and consumer together; preserve only
  externally defined names and terminology.

## Verification

- Run `mise run check` before handoff.
