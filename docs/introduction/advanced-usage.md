---
title: Advanced Usage
sidebar_position: 4
---

# Advanced Usage

Patterns beyond day-to-day install and switch. For a short tour of the common commands, see [Basic Usage](./basic-usage).

## Project version pins

In [shim mode](./modes), NVM for Windows can pick a Node.js version from the current directory.

Supported by default:

- `.nvmrc`
- `.node-version`
- `package.json` (`devEngines.runtime.version`, then `engines.node`)
- `package-lock.json` (shim)

Create or update a pin:

```powershell
nvm rc 22
nvm rc 22 --file=package.json
```

Detection only looks at the **current directory** (no parent walk). Configure the file list with:

```powershell
nvm config set auto_detect=.nvmrc,.node-version,package.json
```

Turn automatic switching off:

```powershell
nvm config set auto_use=0
```

## One-shot version override

Without changing the default, run a single command on another version (shim mode):

```powershell
node --nvm-use 20 script.js
npm --nvm-use=22 test
node --nvm-which --version
```

| Flag | Meaning |
|------|---------|
| `--nvm-use <version>` | Use this version for one invocation |
| `--nvm-which` | Print how the version was resolved |
| `--nvm-shim-version` | Print the shim binary version |

These flags are consumed by the shim and are not passed to Node.js or npm.

## Aliases

```powershell
nvm alias add legacy 18.20.8
nvm alias add modern 24
nvm alias list
nvm use legacy
nvm alias remove legacy
```

Reserved names: `default`, `latest`, `lts`, `last`, `shim`, `link`.

## Auto-install missing versions

When a pin or `nvm use` needs a version that is not installed:

```powershell
nvm config set auto_install=1
nvm config set auto_install_prompt=1
```

With prompts off, missing versions install without confirmation.

## Download cache

Pre-fetch archives (useful offline or on slow links):

```powershell
nvm cache add 20 22
nvm cache view
nvm install 22 --cache
nvm cache remove version 20.19.1
nvm cache remove all
```

Details: [Download Cache](./cache).

## Operating modes

```powershell
nvm use shim    # default — detection, auto-install, security policies
nvm use link    # near zero latency — junction/symlink to active Node
```

Or:

```powershell
nvm config set mode=shim
nvm config set mode=link
```

Full comparison: [Operating Modes](./modes).

## Configuration

```powershell
nvm config list
nvm config get mode
nvm config set cache_downloads=1
nvm config docs
nvm env
```

`nvm env` shows install paths, mode, mirrors, and Node.js runtime security flags. In link mode, security flags are listed but only enforced in shim mode.

## Enable / disable management

```powershell
nvm off    # stop managing Node.js for this user
nvm on     # resume
```

## Security flags (shim mode)

Optional; all default **off**:

```powershell
nvm config set enforce_permission_model=1
nvm config set freeze_v8_global_objects=1
nvm config set disable_eval_and_string_execution=1
```

| Setting | Effect |
|---------|--------|
| `enforce_permission_model` | Prepend Node `--permission` / `--experimental-permission` (default-deny; pass `--allow-*` yourself) |
| `freeze_v8_global_objects` | Prepend `--frozen-intrinsics` |
| `disable_eval_and_string_execution` | Prepend `--disallow-code-generation-from-strings` |

Certified governance builds can force these with ADMX. See [Choosing an Edition](../guide/builds/builds).

## Diagnostics

```powershell
nvm env
nvm doctor
nvm doctor --autofix
```

## Related

- [Command Workflows](./command-workflows) — end-to-end scenarios  
- [Version selectors](./version-selectors) / [specifiers](./version-specifiers)  
- [Commands](/commands)  
