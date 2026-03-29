# Git Metadata Repository

This repository stores structured metadata for the project at:

    git@github.com:entireio/cli.git

It is managed by [gmeta](https://github.com/schacon/gmeta), a tool for associating
key-value metadata with Git objects (commits, branches, paths, and more) and syncing
them across repositories.

## How It Works

Metadata is stored locally in a SQLite database (`.git/gmeta.sqlite`) and serialized
into Git trees and commits under `refs/meta/` refs for synchronization. This
repository serves as the shared remote for that metadata.

You do **not** need to clone this repository directly. Instead, configure it as a
metadata remote in your local checkout of the main project.

## Setup

1. Install gmeta (see [gmeta README](https://github.com/schacon/gmeta) for details).

2. In your local clone of the main project, add this repository as a metadata remote:

   ```
   gmeta remote add git@github.com:schacon/entire-meta.git
   ```

3. Pull existing metadata:

   ```
   gmeta pull
   ```

4. You're ready to read and write metadata:

   ```
   gmeta get commit:HEAD
   gmeta set commit:HEAD review:status "approved"
   gmeta push
   ```

## Contributing Metadata

- **Set values:** `gmeta set <target> <key> <value>`
- **Read values:** `gmeta get <target> [key]`
- **Push changes:** `gmeta push`
- **Pull updates:** `gmeta pull`

Target types include `commit:<sha>`, `branch:<name>`, `change-id:<id>`,
`path:<file>`, and `project` (for repo-wide metadata).

See `gmeta --help` for the full command reference.

## Important Notes

- Metadata is stored on `refs/meta/main`, not on `refs/heads/main`.
  The `main` branch you see here is just this README for orientation.
- Never push directly to `refs/meta/main` — always use `gmeta push`,
  which handles serialization and conflict resolution.
- Metadata can be pruned over time. See `gmeta config:prune` for auto-prune rules.
