# Menu

The Menu application provides the global menu bar in Gershwin. It displays system-wide commands and the active application's menus in a unified menu bar at the top of the screen.

## Features

- **Global menu bar**: Shows menus for the frontmost application
- **Action search**: Search box to quickly find menu commands
- **Multiple toolkit support**: Works with GNUstep apps, GTK apps, and Qt apps
- **D-Bus integration**: Uses the Canonical AppMenu protocol for non-native applications

## Backends

- **GNUstep native**: Uses the Eau theme's Distributed Objects IPC
- **D-Bus AppMenu**: For Qt and other D-Bus aware applications
- **GTK**: Legacy support for Gtk2/3 applications via D-Bus

The Menu bar appears at the top of the screen and updates automatically when you switch applications.
