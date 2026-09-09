---
sidebar_position: 0
title: About (draft)
---

# About

NVM for Windows is a Node.js version manager built for Microsoft Windows. It installs, switches, and removes Node.js runtimes so one machine can use many versions without fighting the official single-install Windows installer.

Since 2014 it has been the common way Windows developers keep LTS, Current, and older lines side by side for apps, CI, and day-to-day work.

## The problem

Node.js on Windows was designed around one installation per machine. The official installer replaces or blocks parallel installs. Real projects do not stay on one line forever: monorepos pin different engines, libraries drop support on EOL releases, and teams still need to reproduce bugs on older runtimes.

Without a version manager, that means reinstalling Node by hand, breaking PATH, and losing time. NVM for Windows owns a version store and puts the active runtime on PATH in a controlled way.

## How it works

At a high level:

1. **Install** downloads a Node distribution into NVM’s root (with optional cache and parallel installs).
2. **Use / default** selects which installed version is active for the shell or system.
3. **Uninstall** removes versions you no longer need.

Two **operating modes** control how `node` / `npm` resolve (see [Operating Modes](./modes)):

| Mode | Idea | Tradeoff |
|------|------|----------|
| **Shim** (default) | Lightweight shims intercept `node` / `npm` / `corepack` | Small Windows process latency; enables per-directory version detection and auto-install |
| **Link** | Junction/symlink to the active `node.exe` | Near zero extra latency; closer to a stock Node install; fewer automatic workflow features |

Shim mode can read project pins such as `.nvmrc`, `.node-version`, and `package.json` engines. Link mode stays useful when you want a static, low-overhead layout (including some air-gapped setups).

Configuration lives under `nvm config` / `nvm cfg`. Day-to-day commands (`install`, `use`, `list`, `alias`, `cache`, `doctor`, and others) are covered in the [command reference](/commands).

## Editions

Same core product; different packaging and controls. Full comparison: [Choosing an Edition](../guide/builds/builds).

| | **Community** | **Certified Builds** |
|--|---------------|---------------|
| **Who** | Individuals and open teams | Organizations that need managed deploy, audit, or policy |
| **License** | MIT (supported for individual workstations) | Commercial EULA |
| **Install** | GitHub / winget `.exe` (Authenticode-signed); program root under LocalAppData | MSI (+ Intune / AD / MECM); program files under Program Files |
| **Management** | Local `nvm config` | Optional structured logging, ADMX policy, licensing by tier |

Community builds ship the full developer feature set without centralized policy. **NVM for Windows Certified Builds** add IT-managed Program Files layout, MSI/Intune packaging, and by tier compliance logging and governance (version allowlists, proxies, air-gap, ADMX, and related enterprise surfaces).

:::tip Supported layout
Community program files are supported under `%LOCALAPPDATA%\Author Software\nvm`. Running Community from another program root is unsupported and may log Application Warning **NVM4101**. Org-standard images and Program Files installs should use **NVM for Windows Certified Builds**.
:::

:::tip Winget / silent install
Community silent install (`/VERYSILENT`) and Winget remain supported. Fleet artifacts (MSI, ADMX, Intune) stay Certified Builds–only.
:::
## What’s different in v2

v2 keeps link mode and adds **shim mode** as the default modern path: per-directory switching, auto-install of missing versions, caching, parallel installs, native extraction, and tighter Windows integrations (notifications, Event Viewer, and related tooling). Details: [What’s new in v2](./newv2).

## Who should use what

- **Solo / OSS / most teams:** Community — install from [GitHub releases](https://github.com/nvm-windows/nvm/releases) or winget, then `nvm install lts`.
- **IT / security / regulated orgs:** **NVM for Windows Certified Builds** — MSI distribution, Program Files layout, and (as licensed) audit or policy packs. See [Deployment](../guide/deploy/deploy).

## Stewardship

NVM for Windows is maintained under Author Software Inc. (project created by Corey Butler). Docs and product site: [docs.nvm-windows.com](https://docs.nvm-windows.com) / [nvm-windows.com](https://nvm-windows.com).
