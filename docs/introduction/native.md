---
title: Native Integrations
sidebar_position: 2
---

NVM for Windows runs natively in Microsoft Windows. Several integrations help the application integrate into the broader Microsoft ecosystem while remaining familiar and friendly for Node.js developers.

## Windows Apps

Installed versions of Node.js are visible in the Windows Apps screen. It is possible to uninstall directly from this screen.

![1776491546067](image/install/1776491546067.png)

## Windows Event Center

Critical events, such as installations, configuration changes, and security events are logged natively. This allows for clear organization observability and auditing using common tools most organizations already have.

![1776532731980](image/native/1776532731980.png)

Only critical change events are logged by default. Additional logging is available through configuration. It is possible to log every node.exe/npm/npx invocation.

:::warning Basic Logging
The **community** and **certified distribution** editions write plaintext entries to the Windows Application log (shown above), with generic event codes.

If the NVM for Windows community installer is prevented from registering itself as a Windows event source, Application log entries are written as an "unknown" event source instead of "NVM for Windows". This is not an issue in certified distribution builds.
:::

:::tip SIEM/Audit Logging
Certified **audit/governance editions** write structured entries, with well known SIEM event codes, to a dedicated native NVM for Windows log (not the Application log). This is designed for streamlined SIEM integration and simple querying.
:::

## Windows Notification Center

NVM for Windows leverages native desktop notifications through the notification center. Missed notifications will be available in the notification center until acknowledged.

Since all notifications leverage the notification center, personal notification preferences are honored.

<img
	src={require('./image/native/1776541489737.png').default}
	alt="Windows Notification Center"
	style={{width: '40%', maxWidth: '480px'}}
/>

## Windows Registry

As of v2.0.0, settings and preferences are stored in the registry under user keys. These can be modified with the [`nvm config`](../command/config) command.

:::info Attention International v1 Users
Prior versions of NVM for Windows utilized a plain text `settings.txt` file. Some users experienced difficulties using special characters caused by encoding types enforcement in older versions of Go. Windows handles locale encoding natively in the registry, eliminating this problem.
:::

:::tip Enterprise Security
NVM for Windows provides significant capabilities for developers. In highly regulated environments, some of these capabilities may need to be throttled or disabled for compliance. **Certified builds** provide an option to override/enforce registry settings, enabling organizations to secure desktop environments according to their own policies.
:::