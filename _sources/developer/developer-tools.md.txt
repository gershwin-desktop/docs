# Developer Tools

## Source tree layout

The Gershwin source tree is organized under `/Developer/Library/Sources/`:

| Directory | Purpose |
|---|---|
| `gershwin-desktop/` | Live ISO build definitions |
| `gershwin-developer/` | Core development environment, bootstrapping scripts |
| `gershwin-components/` | Applications and system services (Menu, LoginWindow, etc.) |
| `gershwin-workspace/` | File manager and desktop |
| `gershwin-windowmanager/` | Native X11 window manager |
| `gershwin-systempreferences/` | System Preferences app and preference panes |
| `gershwin-terminal/` | Terminal emulator |
| `gershwin-textedit/` | Text editor |
| `gershwin-eau-theme/` | GNUstep theme with global menu integration |
| `gershwin-system/` | System-level scripts and tools |

## Build system

Gershwin uses GNUstep make (`tools-make`). Key targets in `gershwin-developer`:

| Target | Description |
|---|---|
| `make corelibs` | Build libobjc2, GNUstep base/gui/backend, and system infrastructure |
| `make workspace` | Build gershwin-workspace |
| `make windowmanager` | Build gershwin-windowmanager |
| `make components` | Build gershwin-components (Menu, LoginWindow, etc.) |
| `make systempreferences` | Build gershwin-systempreferences |
| `make terminal` | Build gershwin-terminal |
| `make textedit` | Build gershwin-textedit |

## Starting Gershwin

After installation, start Gershwin with:

```console
startx /System/Library/Scripts/Gershwin.sh
```

Or use the login window:

```console
/System/Library/Scripts/LoginWindow.sh
```

## Testing

- **Unit tests**: Run `gnustep-tests` from a component directory
- **UI testing**: Use the `driveui` tool to drive running GNUstep apps
- **UIBridge**: MCP server for programmatic UI inspection and automation (see `developer/uibridge`)

## Debugging

- **DTrace/dtruss**: Available on FreeBSD; see `developer/tracing`
- **LLDB**: Use `lldb_exec` via UIBridge for deep process inspection
- **Logs**: UIBridge writes diagnostics to `/tmp/uibridge.log`

## Resources

- [GNUstep developer documentation](http://developer.gnustep.org/)
- [Gershwin Discussions](https://github.com/orgs/gershwin-desktop/discussions)
- `#gershwin` on irc.libera.chat
