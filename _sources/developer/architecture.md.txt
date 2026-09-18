# Architecture

Gershwin is designed with simplicity in mind. Less, but better. This page describes key elements of the Gershwin desktop. Everything should be welcoming to switchers from other operating systems but ideally even easier to use.

## Frameworks

GNUstep is the core application framework. All native Gershwin applications are written in Objective-C using GNUstep.

## Desktop components

The minimum number of components viable to produce an efficient desktop should be used. Each component should be as simple as possible, and have as few dependencies as possible (besides the ones that are central to Gershwin, such as GNUstep). XDG specifications are considered overly complex but insufficient and should be avoided; they may be acceptable as legacy technology for compatibility reasons.

### Workspace

File manager that manages `.app` bundles and provides the Desktop. Contains the Dock (application launcher), desktop icons, and file browsing. Source: `gershwin-workspace/Workspace/`.

### Menu

Global menu bar that displays system-wide commands and the active application's menus. Implements native GNUstep menu export via the Eau theme, Canonical AppMenu via D-Bus, and legacy Gtk2/3 menu support. Source: `gershwin-components/Menu/`.

### WindowManager

Native X11 window manager written for Gershwin. Handles window positioning, resizing, focus management, compositing, and window switching (Overview). Source: `gershwin-windowmanager/WindowManager/`.

### LoginWindow

Graphical login screen. Source: `gershwin-components/LoginWindow/`.

### Terminal

Terminal emulator application. Source: `gershwin-terminal/`.

### TextEdit

Text editor application. Source: `gershwin-textedit/`.

### SystemPreferences

System settings application with pluggable preference panes. Source: `gershwin-systempreferences/SystemPreferences/`.

### UIBridge

MCP server for automating and inspecting GNUstep applications at runtime. Source: `gershwin-components/UIBridge/`.

### initgfx

Automatic graphics hardware configuration. Source: `gershwin-components/initgfx/`.

## Applications

Applications must not need to be installed. Simply downloading them, attaching an external drive containing them, or connecting to a network share containing them must be sufficient. Multiple versions of the same application must be able to co-exist.

Custom-written applications should come as application bundles whenever possible. It is acceptable for pre-existing applications to come with legacy XDG desktop files instead.

## Utilities

Gershwin comes with commonly used utilities, such as a Terminal application, a Process Monitor application, etc. These are just regular applications bundled with the system.

## Preferences

Gershwin comes with preference panels for commonly used settings, such as for configuring the network, keyboard, display, and other system options.
