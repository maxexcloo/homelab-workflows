# Improvement Plan

## Priority 1: Protect Library Operations

### Make ingest publication recoverable and accurately documented

- Decide whether ingest requires true batch atomicity.
- If true batch atomicity is required, build a complete replacement tree on the
  same filesystem and publish it with one directory or symlink switch.
- If per-file publication is retained, describe it accurately in `README.md` and
  add a journal or resumable publication process so partial failures can be
  recovered safely.
- Use a run-specific validation directory instead of blocking retries whenever a
  batch-named validation directory already exists.
- Clean abandoned staging data after validation failures while preserving reports
  needed for diagnosis.
- Reject empty batches explicitly.

### Pin every executable and reviewed input

- Pin the Fresh1G1R checkout to a reviewed commit.
- Pin IGIR to an exact version rather than the floating major version `5`.
- Pin third-party GitHub Actions to reviewed commit SHAs and let Renovate propose
  controlled updates.
- Record the reviewed DAT revision alongside the games configuration so a workflow
  run can be reproduced from its repository commit.

## Priority 2: Make Safety Gates Complete

### Apply non-DAT manifest verification consistently

- Extract the manifest verification logic into equivalent direct steps in both
  audit and reconcile.
- Verify each manifest's own SHA-256 digest before checking the files it lists.
- Ensure reconciliation fails when any reviewed non-DAT file is absent, changed,
  or unexpectedly present.
- Confirm how IGIR reports each configured non-DAT format and prevent reviewed
  non-DAT content from being rejected merely as `UNUSED`.

### Make configuration authoritative

- Read configured library, staging, report, restore-confirmation, and export paths
  from `config/games.json` instead of repeating literals in the workflow.
- Use `canonical_seed`, source names, format lists, export definitions, and
  `tested_romm_version` where they govern behaviour.
- Remove fields that remain only speculative or reserved until their associated
  workflow mode is implemented.
- Validate `config/games.json` against a repository-owned JSON Schema so required
  fields, hashes, paths, unique source names, and supported selections are checked
  before a runner touches the library.

## Priority 3: Improve Operational Reliability

### Establish runner prerequisites explicitly

- Create or validate report, processed, staging, and validation directories before
  using them.
- Check that source paths exist and are readable before invoking IGIR.
- Check that publication source and destination paths are on the expected
  filesystem before relying on rename semantics.
- Add suitable `timeout-minutes` values to every self-hosted job.
- Emit concise failure messages for confirmation, missing-directory, collision,
  and restore-evidence checks instead of relying on a bare `test` exit status.

### Strengthen publication checks

- Detect destination changes between collision validation and publication.
- Produce a manifest of the validated batch before publication and verify the
  published result against it afterward.
- Mark a batch processed only after every expected destination has been verified.
- Document the manual recovery procedure until publication is fully resumable.

## Priority 4: Tighten Repository Checks

### Add deterministic shell validation

- Add a pinned ShellCheck tool to `.mise.toml` so Actionlint does not silently
  disable embedded-shell analysis during local checks.
- Add an explicit ShellCheck-oriented hook if Actionlint alone does not provide
  sufficient visibility into multiline workflow scripts.
- Keep `mise run check` as the single local and continuous-integration entry point.

### Resolve small consistency issues

- Sort unordered JSON object keys according to `AGENTS.md`, including source and
  export objects.
- Use one consistent approach to creating report directories across all jobs.
- Consider running the Prek workflow on pushes to the default branch if branch
  protection does not already guarantee pull-request validation.

## Suggested Delivery Order

1. Pin actions, IGIR, and the DAT revision.
2. Make failed ingest runs recoverable and correct the atomicity documentation.
3. Apply manifest verification to reconciliation and define non-DAT handling.
4. Make `config/games.json` authoritative or remove its unused fields.
5. Add runner timeouts, prerequisite checks, and post-publication verification.
6. Pin ShellCheck and resolve the remaining ordering and consistency items.

## Completion Criteria

- A workflow run is reproducible from one repository commit.
- Audit and reconcile verify identical reviewed non-DAT evidence.
- Changing a configured path changes every relevant workflow operation.
- An interrupted ingest can be retried without manual deletion or duplicate
  publication.
- Documentation describes the actual publication guarantee.
- Self-hosted jobs cannot hold the runner or concurrency group indefinitely.
- `mise run check` includes ShellCheck-backed analysis and passes.
