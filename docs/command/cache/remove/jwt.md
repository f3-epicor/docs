---
title: jwt
sidebar_position: 4
---

# nvm cache remove jwt

Clear the in-process cached Author mirror license JWT (`X-Author-License`).

**Alias:** `license`

## Usage

```powershell
nvm cache remove jwt
nvm cache remove license
```

## Arguments

This subcommand takes no arguments.

## Flags

This subcommand has no flags.

## Example

```powershell
nvm cache remove jwt
```

## Notes

- The mirror license JWT is cached in-process for about 15 minutes (refreshed about 1 minute before expiry).
- The JWT `versions` claim includes magic keywords plus concrete allow-list versions resolved from policy (for example `["NOT EOL","14.21.3"]`).
- Clearing forces the next Author-mirror request in this process to mint a fresh token.
- Community builds have no license JWT cache; the command reports nothing to clear.
- Download-cache removal policy (`allow_download_cache_removal`) does not block this command.
