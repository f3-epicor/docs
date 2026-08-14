---
title: Basic Usage
sidebar_position: 3
---

# Basic Usage

After [installation](./installation), these commands cover everyday Node.js version management.

## Install a version

```powershell
nvm install lts
nvm install 22
nvm install 20.19.1
```

Install several at once:

```powershell
nvm install 18 20 22
```

`lts` and `latest` resolve against the Node.js release list. See [version specifiers](./version-specifiers) for more forms.

## Switch the default version

```powershell
nvm use 22
nvm use lts
node -v
```

`nvm use` updates the system default managed by NVM for Windows. In [shim mode](./modes) (the default), you can also rely on per-project detection without changing the default every time.

## List what you have

```powershell
nvm list
nvm list releases
nvm default
```

- `list` — installed versions  
- `list releases` — available releases from the configured mirror  
- `default` — current default version  

## Uninstall

```powershell
nvm uninstall 18.20.8
nvm uninstall 20.*
```

## First-run checklist

```powershell
nvm install lts
nvm use lts
node -v
npm -v
nvm env
```

If something looks wrong, run `nvm doctor` or see [Command Workflows](./command-workflows).

## Next steps

- [Advanced Usage](./advanced-usage) — aliases, project pins, cache, modes, config  
- [Operating Modes](./modes) — shim vs link  
- [Commands](/commands) — full command reference  
