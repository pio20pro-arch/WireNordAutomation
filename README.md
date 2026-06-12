# WireNordAutomation

Windows 11 GUI for managing NordVPN WireGuard connections through the local WireGuard installation at `C:\Program Files\WireGuard`.

This project exists because the NordVPN Windows app is unnecessarily heavy for this use case. WireNordAutomation focuses on automating connection management, server selection, and a few basic controls without the extra baggage.

It was initially inspired by the NordVPN WireGuard config generator project, but the current codebase is a separate Windows 11 GUI app with its own implementation and workflow.
Reference: https://github.com/mustafachyi/NordVPN-WireGuard-Config-Generator

<img width="1508" height="1676" alt="image" src="https://github.com/user-attachments/assets/160dd6b2-9447-492a-9253-0252f81f1e44" />


## Important paths

- Settings: `%AppData%\WireNordAutomation\settings.json`
- Encrypted token: `%AppData%\WireNordAutomation\auth.sec`
- Logs: `%AppData%\WireNordAutomation\logs`
- Tunnel config: `%ProgramFiles%\WireGuard\Data\Configurations\wirenordautomation.conf`
