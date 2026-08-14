# Installation

NVM for Windows is publicly available through GitHub and Winget. If your organization supports certified builds, it will likely be automatically installed on your computer.

### Download

[Download](https://github.com/nvm-windows/nvm/releases) and run the installer. Then install Node.js with the following command:

```powershell
nvm install lts
```
:::info This is the recommended approach.
The installer visually walks through the configuration, allowing users to customize their experience according to their preference.
:::

### Winget

Use winget for a silent installation using default configurations.

```powershell
winget install nvm
nvm install lts
```

### Upgrade from v1

Just [download](https://github.com/nvm-windows/nvm/releases) and install. The installer automatically migrates v1 to v2.

:::warning Legacy Updater
The v1 updater is designed for minor and patch upgrades in the legacy v1.x.x line. It will not work with v2.
:::