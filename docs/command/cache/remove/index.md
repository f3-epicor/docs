---
title: remove
sidebar_position: 3
---

# nvm cache remove

Remove cached artifacts.

**Alias:** `rm`

## Subcommands

| Subcommand | Description |
|------------|-------------|
| [`version`](./version) | Remove cached Node.js archives (default). |
| [`metadata`](./metadata) | Remove cached metadata files. |
| [`jwt`](./jwt) | Clear the cached mirror license JWT. |
| [`all`](./all) | Clear metadata, versions, and mirror license JWT caches. |

## Notes

Removal can be blocked when `allow_download_cache_removal=false`.
