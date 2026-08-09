# Homelab Workflows

This repository contains reviewed configuration and manually dispatched
maintenance workflows that do not belong to one deployment platform. The
current workflow audits and reconciles the self-hosted games library used by
RomM and related clients.

## Games Workflow

The `Games` workflow runs on the `romm-workflows` self-hosted runner and uses
Fresh1G1R DATs with IGIR to identify canonical content.

| Mode        | Purpose                                                                                         |
| ----------- | ----------------------------------------------------------------------------------------------- |
| `audit`     | Check the canonical library and reject content outside the reviewed DATs.                       |
| `export`    | Reserved for the RomM 5 export adapter; currently fails closed until compatibility is verified. |
| `ingest`    | Validate one inbox batch, reject unknown files or collisions, then publish it atomically.       |
| `inventory` | Report how the configured source directories match the reviewed DATs.                           |
| `reconcile` | Build a verified `library.next` from all configured sources without replacing the live library. |

Run it from **Actions → Games → Run workflow**. Ingest requires an inbox batch
name. Ingest and reconcile also require the exact confirmation
`PUBLISH VERIFIED ROMS`.

## Safety Model

- `config/games.json` records reviewed source paths, preserved formats, output
  paths, and non-DAT manifest hashes.
- Ingest rejects an entire batch when any file is unknown and refuses conflicting
  canonical destinations.
- Reconciliation requires confirmed application-consistent restore evidence, a
  populated non-DAT manifest list, and an empty `library.next` directory.
- Reports and staging data remain on the self-hosted runner under
  `/games/workflow/`; they are not committed or uploaded as public artefacts.

The empty `non_dat_manifests` list currently keeps audit and reconcile closed.
Ingest remains limited to DAT-covered batches until those additional sources
have been reviewed and recorded.

## Repository Layout

- `.github/workflows/games.yaml` — guarded games-library operations.
- `config/games.json` — reviewed paths, formats, sources, and safety evidence.

## Development

```bash
mise run setup  # Install Git hooks
mise run check  # Run all repository checks
mise run fmt    # Format supported files
```

## Licence

AGPL-3.0 - see [LICENSE](LICENSE).
