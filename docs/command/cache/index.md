---
title: cache
sidebar_position: 9
---

# nvm cache

View and manage cached install assets.

## Usage

```powershell
Usage: nvm cache <command>
```

## Subcommands

| Subcommand | Aliases | Description |
|------------|---------|-------------|
| [`add`](./add) | — | Download and cache without installing. |
| [`view`](./view) | `ls` | List cache stores and files. |
| [`remove`](./remove/) | `rm` | Remove cached artifacts. |

## Examples

```powershell
nvm cache view
nvm cache add 24
nvm cache remove version 24.1.0
nvm cache remove jwt
nvm cache remove all
```

## Notes

- Removal of download archives/metadata can be blocked by policy (`allow_download_cache_removal=false`).
- [`nvm cache remove jwt`](./remove/jwt) clears the in-process Author mirror license JWT cache and is not gated by that policy.

See [Download Cache](../../guide/cache).
