# Boot Process

Gershwin boots via the underlying operating system's init system, then starts the Gershwin session.

## System preparation

`/System/Library/Scripts/SystemPrepare.sh` configures the system to boot into Gershwin Desktop. It sets up the environment, prepares display managers, and configures autologin if desired.

## Session startup

On login, the session is started via one of:

- `startx /System/Library/Scripts/Gershwin.sh` (manual X start)
- `/System/Library/Scripts/LoginWindow.sh` (launches login window, which starts X)
- On FreeBSD/GhostBSD: `service loginwindow start`

## LoginWindow

The `LoginWindow` app (`gershwin-components/LoginWindow/`) provides the graphical login screen. It handles user authentication and starts the user's Gershwin session.

## Gershwin.sh

`/System/Library/Scripts/Gershwin.sh` is the session startup script. It:
- Sources the GNUstep environment (`GNUstep.sh`)
- Starts the WindowManager
- Starts the Menu bar
- Starts Workspace
- Starts other session components

## Monkey patching

For rapid development, the live ISO boot process supports monkey patching. See `developer/monkey-patch` for details.
