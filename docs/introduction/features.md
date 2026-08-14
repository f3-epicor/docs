---
title: v2 feature outline
sidebar_position: 4
---

# v2 feature outline

Working table of contents for describing NVM for Windows 2. Built from the CLI, shim, settings, and certified policy surfaces — not from marketing copy.

**Legend:** Community (C) · Distro (D) · Audit (A) · Governance (G) · Custom = G + contract.

Existing pages in parentheses. Gaps have none.

---

## 1. Product and editions

1. What NVM for Windows is — multi-version Node on Windows ([About](./about), [draft](./about-alt))
2. Community vs certified — MIT `.exe` vs signed MSI, who each is for ([Choosing an Edition](../guide/builds/builds))
3. Certified tiers — Distro / Audit / Governance / Custom (same app; packaging + gates differ)
4. v1 → v2 — no `NVM_HOME` / `NVM_SYMLINK`; registry instead of `settings.txt`; unsigned community installer / SmartScreen ([What’s new in v2](./newv2))

## 2. Operating modes

1. Shim vs link — when to pick which ([Operating Modes](./modes))
2. Shim mode (default) — Zig `node` / package-manager / `reshim` interceptors; ~25–35 ms Windows process tax
3. Link mode — `.nodejs` junction, symlink fallback, `SeCreateSymbolicLinkPrivilege` / Developer Mode; no UAC elevation
4. Enable / disable — `nvm on` / `nvm off` (`enabled`)

## 3. Version selection

1. Specifiers — `latest`, `lts`, `lts/<codename>`, `x` / `x.y` / `x.y.z`, semver ranges (`^18`, `>=16`, `~20.1`, `*`, `18.x`) ([version specifiers](./version-specifiers), [selectors](./version-selectors))
2. Aliases — `nvm alias add|list|remove`; reserved `default` / `latest` / `lts` / `last` / `shim` / `link`
3. Per-directory detection (shim) — cwd only, no parent walk; default `.nvmrc`, `.node-version`, `package.json` (`engines.node`; shim also `devEngines.runtime.version`); shim extra `package-lock.json`; `auto_detect` / `auto_use`
4. Run-command files — `nvm rc` writes the default detect file
5. `nvm use` — version / `lts` / `latest` / `last` / `shim` / `link`; `--local`; auto-install of missing versions
6. `nvm default` / `current` — show active version
7. Minimum versions — Node ≥ 10.8.0; ARM64 ≥ 20

## 4. Install, uninstall, cache

1. Parallel install — `nvm install` multiple versions at once
2. 7z sources + native extract — `node-v*-win-*.7z`; pinned `7z.exe` then Go fallback
3. Integrity — SHASUMS256, Authenticode, `allowed_signers`
4. Flags — `--force`, `--cache` / `--no-cache`, `--insecure`, `--copy-from` / `--from`
5. Default global modules — `auto_installed_modules` after each new version
6. Native tools — `nvm install native-tools` → `install_tools.bat` (`allow_tool_install`)
7. Uninstall — parallel, wildcards (`22.*`), `*` / `all` with confirm, `--purge`
8. Rollback — Ctrl+C restores in-progress install
9. Download cache — `.cache/versions`; `nvm cache add|view|remove` (`all` / `metadata` / `jwt` / version); `cache_downloads`, `allow_download_cache_removal` ([Download Cache](./cache) — stub)
10. Offline / local archives — `local_dir`, `local_install_only`

## 5. Shim workflows

1. Intercepted commands — `node`, `npm`, `npx`, `pnpm`, `yarn`, `corepack`
2. Shim flags — `--nvm-use`, `--nvm-which`, `--nvm-shim-version`
3. Auto-install missing runtime — `auto_install`, `auto_install_prompt`
4. Verify cache — `{DataRoot}/.verify` + HKCU; miss → full Authenticode
5. Package-manager mismatch — `pm_mismatch_action` vs `devEngines.packageManager` (`ignore` / `warn` / `error`)
6. Reshim — globals / `-g` / yarn global / `corepack enable`; hidden `nvm reshim`
7. Execution audit (optional) — `log_executions` → Event Log on each node invoke

## 6. Configuration and environment

1. `nvm config` — `list` / `get` / `set` / `reset` / `docs`; `key=value`; `--json`
2. Registry layout — HKCU prefs vs HKLM policy (`Software\Author Software\Preferences\nvm`, `Software\Policies\Author Software\nvm`) ([Native Integrations](./native))
3. Install root — `root` (settable; hidden from list/docs); `allow_root_dir_change`
4. Mirrors — `node_mirror` / `npm_mirror` (comma-separated)
5. Proxy (all editions) — `proxy` → env → IE `ProxyEnable`; basic / bearer
6. `nvm env` — install paths, mode, active version
7. Secrets not via `cfg` — `access_token` / `access_key` / `active_version` (license / `use`)

## 7. Windows integrations

1. Windows Apps — per-version Uninstall keys (`nvm4w-node-v*`); uninstall from Apps
2. Event Viewer — provider `NVM for Windows`; Community/Distro plaintext Application log; Audit/Governance structured ETW ([log](./log))
3. Notification Center — WinRT toasts; personal notification prefs honored ([notifications](./notifications))
4. Scheduled sync — hourly user task `NVM for Windows Sync`
5. v1 cleanup — strip SYSTEM `NVM_HOME` / `NVM_SYMLINK`; migrate App Paths / tasks
6. PATH — `{DataRoot}\.nodejs` (no env-var dance)

## 8. Maintenance and diagnostics

1. `nvm doctor` — checks, `--autofix`, `--list`, `--json`
2. `nvm upgrade` — self-update; `--check`; `disable_upgrade`
3. Announcements — news/releases toasts; `disable_announcements`; `nvm subscribe`
4. Hidden installer switches — event-log register/unregister, migrate installed versions, cleanup AppData, remove sync tasks, seed announcement watermarks

## 9. Security (all editions)

1. Authenticode on `node.exe` before spawn (shim) and on install
2. Allowed publisher names — `allowed_signers`
3. TLS — `allow_insecure_downloads` / `--insecure`
4. Air-gapped license crypto — `air_gapped` skips live JWKS (sidecar / `JwksCose`)
5. Shim directory hardening — certified tree ACL on `.shim` / `.sync` (community still verifies binaries)
6. Node permission model (opt-in, shim) — `enforce_permission_model` / `EnforcePermissionModel`; prepends `--permission` (23+) or `--experimental-permission` (20–22); default off; Governance ADMX can force on
7. Freeze V8 intrinsics (opt-in, shim) — `freeze_v8_global_objects` / `FreezeV8GlobalObjects`; prepends `--frozen-intrinsics` (Node 12+); default off; measurable latency
8. Disallow string-to-code (opt-in, shim) — `disable_eval_and_string_execution` / `DisableEvalAndStringExecution`; prepends `--disallow-code-generation-from-strings`; default off

## 10. Certified Distro

1. EV-signed MSI + MST (`ACCEPT_EULA=1`) + `.intunewin` / `.intune.json`
2. AccessToken + JwksCose licensing (no AccessKey) — `Set-NvmWindowsAccessToken.ps1`
3. Same developer feature set as Community; no ADMX; plaintext logs

## 11. Certified Audit

1. Everything in Distro
2. Structured SIEM logging — dedicated channels, queryable event IDs (installs, config, executions)
3. Still no ADMX / AccessKey / IWA

## 12. Certified Governance (Custom = this + contract)

1. ADMX/ADML — 33 machine policies (enabled, root, mode, detect, network, security)
2. Version allow/block lists — local lists; Author-mirror JWT magic (`EOL`, `ALPHA`, `MAINTENANCE`, `ALL`)
3. Advanced proxy — IWA (NTLM/Negotiate), PAC/WPAD; Governance-only
4. Author Node mirror — `AccessKey` → `X-Author-License`; 15-minute mirror JWT
5. Package cooldown — `NpmModuleMinimumAge` (shim; npm days / pnpm minutes / yarn age gate)
6. Node permission model — `EnforcePermissionModel` (shim; default off; prepends `--permission` / `--experimental-permission`; no `--allow-*` grants)
7. Freeze V8 intrinsics — `FreezeV8GlobalObjects` (shim; default off; `--frozen-intrinsics`)
8. Disallow string-to-code — `DisableEvalAndStringExecution` (shim; default off; `--disallow-code-generation-from-strings`)
9. Policy deploy — GPO Central Store, Entra ingest, Google Workspace CSV, remediation startup scripts
10. Licensing scripts — `Set-NvmWindowsLicensing.ps1` (token + key + JwksCose)
11. Event-log provider + legacy env cleanup — `Remediation/`
12. Trust pack (separate zip, entitled by Governance license) — CycloneDX SBOM (embedded JSF), SLSA Build L3 provenance, verify docs
13. Expiry notices — toasts at 7 / 3 / 1 / day-of / 7-day grace

## 13. Command index (docs already exist)

| Area | Commands |
|------|----------|
| Install | `install`, `install native-tools`, `uninstall` |
| Switch | `use`, `use lts\|latest\|last\|shim\|link`, `default`, `on`, `off` |
| Discover | `list`, `list releases`, `list cached`, `rc`, `alias` |
| Store | `cache add\|view\|remove` |
| Config | `config list\|get\|set\|reset\|docs`, `env` |
| Ops | `doctor`, `upgrade`, `subscribe`, `license` (hidden), `reshim` (hidden) |

---

### Not in the product (do not document as shipped)

- Parent-directory walk for `.nvmrc` — resolver is **cwd only**.
- Digest proxy auth.
- Injecting `--allow-*` grants with the permission model — NVM only prepends the enable flag; orgs/users supply grants at runtime.

### Suggested write order

1. Modes + version detection (users hit this first)
2. Install / cache / offline
3. Native Windows (Apps, log, toasts)
4. Config reference (one page per settings group)
5. Editions + Distro/Audit/Governance deploy
6. Governance policy catalog (map each ADMX to a short how-to)
