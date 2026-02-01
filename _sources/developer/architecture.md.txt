# Architecture

Gershwin is designed with simplicity in mind. Less, but better. This page describes key elements of the Gershwin desktop. To begin with, everything should be welcoming to switchers from other operating systems but ideally even easier to use.

## Frameworks

GNUstep

## Desktop components

The minimum number of components viable to produce an efficient desktop should be used. Each component should be as simple as possible, and have as few dependencies as possible (besides the ones that are central to Gershwin, such as GNUstep). Outside dependencies that are closely tied to other outside components should be avoided. XDG specifications are considered overly complex but insufficient and should be avoided, but may be acceptable as legacy technology for compatibility reasons.

### Workspace

File manager that can handle `.app` bundles and also provides the Desktop.

### Menu

Global menu bar that displays the Command menu for system-wide commands, and the active application's menus. It also contains a search box to search in the menus.

### Dock

A Dock that shows icons for running and pinned applications. (In the future it should become its own window, for now it is part of the Desktop window provided by Workspace.)

### IPC mechanism for the desktop

GNUstep IPC is used, e.g., in the Eau theme for exporting the menus of GNUstep applications to Menu.app.

### Applications

Applications must not need to be installed. Simply downloading them, attaching an external drive containing them, or connecting to a network share containing them must be sufficient. Multiple versions of the same application must be able to co-exist.

Custom-written applications should come as Application bundles whenever possible. It is acceptable for pre-existing applications to come with legacy XDG desktop files instead.

### Utilities

Gershwin comes with some commonly used utilities, such as a Terminal application, a Process Monitor application, etc. These are just regular applications.

### Preferences

Gershwin comes with commonly used preference panels, such as for configuring the network, keyboard, etc.
