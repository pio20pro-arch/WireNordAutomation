# WireNordAutomation

WireNordAutomation is a lightweight Windows 11 desktop application for managing NordVPN connections through a local [WireGuard](https://www.wireguard.com/) installation. It provides server selection, connection automation, firewall-based leak protection, split tunneling for selected IP ranges, and connection diagnostics without requiring the official NordVPN desktop client. That recently contain even build-in antivirus.

Current version: **0.1.1**

> [!IMPORTANT]
> WireNordAutomation is an independent, unofficial project. It is not developed, supported, or endorsed by NordVPN or WireGuard.

# Classic UI 
<img width="50%" height="50%" alt="image" src="https://github.com/user-attachments/assets/87fe4acb-0776-4077-9f1d-ef598ae2a788" />

# React UI (beta)
<img width="50%" height="50%" alt="image" src="https://github.com/user-attachments/assets/4be71234-e019-464a-9c8f-5bbc5603c48f" />


## Features

### VPN connection management

- Connect to a selected NordVPN server or automatically choose the best matching server.
- Filter servers by country, city, and category: Standard, P2P, Dedicated IP, Onion, or Double VPN.
- Connect using an endpoint IP, a hostname, or hostname-to-IP fallback mode.
- Reconnect without reopening the application and disconnect directly from the window or tray menu.
- Verify a recent WireGuard handshake before reporting a new connection as successful.
- Reuse the securely stored NordLynx private key instead of contacting the NordVPN API on every reconnect.

### Server catalog and latency

- Download and cache the NordVPN server catalog.
- Reuse a catalog younger than 12 hours unless a refresh is requested manually.
- Measure ICMP latency for low-load servers in the active dashboard filter.
- Browse and search the complete cached catalog in React UI.
- Ping one server or all server cards currently visible in the catalog.
- Sort server results using load and measured latency; failed measurements are displayed as timeouts instead of artificial latency values.

### Kill switch and excluded networks

- Apply a kill switch using Windows Defender Firewall outbound policy and dedicated allow rules.
- Keep the WireGuard endpoint and tunnel interface traffic available while blocking unrelated outbound traffic.
- Return to block-only mode when the tunnel is disconnected and the kill switch remains enabled.
- Allow specified IPv4 addresses or subnets to bypass the VPN, for example local network devices.
- Use custom DNS resolver addresses in generated WireGuard configurations.

> [!WARNING]
> The kill switch changes Windows Firewall policy for all profiles and requires administrator privileges. Incorrect external firewall rules can block network access. Use the application's kill switch control to restore normal outbound access.

### Startup and background operation

- Run from the Windows notification area and minimize to the tray when the window is closed.
- Start WireNordAutomation automatically after user sign-in.
- Choose whether React UI starts hidden in the tray.
- Choose whether the WireGuard tunnel service starts automatically with Windows.
- Use Always Connect to re-establish a missing connection in the background.
- Switch between the classic WinForms UI and React UI from the tray menu without restarting the application.

### React UI tools

- View live transfer rates, transferred data, interface MTU, and WireGuard handshake age.
- View, filter, copy, clear, and export application logs.
- Generate a WireGuard `.conf` profile for a selected cached NordVPN server, then copy or download it.
- Select a dark or light interface theme.
- Configure the WireGuard executable path.
- Create and test automation rules for process starts, untrusted Wi-Fi networks, tunnel failures, and daily schedules.
- Use automation actions to connect, disconnect, reconnect to the fastest matching server, or terminate a configured process after a tunnel drop.

## Requirements

- Windows 11
- Administrator privileges
- [.NET 8 Desktop Runtime](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)
- [WireGuard for Windows](https://www.wireguard.com/install/)
- Microsoft Edge WebView2 Runtime for React UI
- A valid NordVPN subscription and NordVPN access token

The default WireGuard executable path is:

```text
C:\Program Files\WireGuard\wireguard.exe
```

The path can be changed in React UI settings.

## Getting started

1. Install WireGuard for Windows.
2. Start WireNordAutomation as an administrator.
3. Paste your NordVPN access token. The token is encrypted for the current Windows user before it is stored.
4. Select a country, city, server category, and endpoint connection mode.
5. Review the DNS and excluded IP settings.
6. Select a server or use **Reconnect to Best**.
7. Enable the kill switch only after reviewing the warning above.

The application continues running in the notification area when its window is closed. Right-click the tray icon to show or hide the active UI, refresh servers, reconnect, disconnect, switch UI, or exit completely.

## Configuration notes

### Access token

Both interfaces use the same encrypted token store. Changing the token in React UI also changes the token used by the classic UI. The token is stored with Windows Data Protection API protection scoped to the current Windows account.

### DNS

The DNS field is editable. Its value is written to the next WireGuard configuration. The default value is `49.12.222.213`.

### Excluded IP addresses

Enter individual IPv4 addresses or CIDR subnets separated by commas:

```text
192.168.0.1, 192.168.31.0/24
```

Changes made while connected require a reconnect before they become active. A single IP address remains a single host; it is not automatically expanded to a `/24` subnet.


## Data locations

| Data | Path |
| --- | --- |
| Application settings | `%AppData%\WireNordAutomation\settings.json` |
| Encrypted token and NordLynx key | `%AppData%\WireNordAutomation\auth.sec` |
| Server catalog cache | `%AppData%\WireNordAutomation\servers-cache.json` |
| Automation rules | `%AppData%\WireNordAutomation\automation-rules.json` |
| Daily logs | `%AppData%\WireNordAutomation\logs\yyyyMMdd.log` |
| Active tunnel configuration | `%ProgramFiles%\WireGuard\Data\Configurations\wirenordautomation.conf` |

## Troubleshooting

- Check the latest file in `%AppData%\WireNordAutomation\logs` when a connection, ping, automation, or firewall operation fails.
- Confirm that `wireguard.exe` and `wg.exe` exist in the configured WireGuard installation directory.
- If the WireGuard service is running but the application rejects the connection, inspect the log for handshake verification and firewall-rule errors.
- If the kill switch blocks all traffic after a failed connection, disable it from the application to restore the standard outbound firewall policy.
- If Tailscale is installed, confirm that an enabled outbound rule named `Tailscale-Process` exists.

## Background

WireNordAutomation was created as a focused alternative for users who want NordLynx connectivity through WireGuard with explicit control over server selection, firewall behavior, and automation.

The project was initially inspired by [NordVPN WireGuard Config Generator](https://github.com/mustafachyi/NordVPN-WireGuard-Config-Generator).
