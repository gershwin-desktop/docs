# Global Menu

Gershwin intends to provide a unified user experience for all applications regardless of toolkit being used by application developers.

To provide this unified user experience, it is crucial that all applications can show their commands in the system-wide global menu.

## Terminology

:::{note}
Consistent, up-to-date documentation regarding the subject of global menus is hard to find. 

Please consider submitting issues and pull requests if you can contribute to this subject.
:::

### Concepts 

* __Global menu bar__: A concept in user interface design where a system-wide widget on the screen displays the menu items (also known as "actions" in Qt) for all applications
* [__Menu__](https://github.com/gershwin-desktop/gershwin-components/Menu/): The application in Gershwin that shows the global menubar

### APIs

Gershwin Desktop currently supports

* Native GNUstep applications: The Eau theme exports menus via native GNUstep IPC mechanisms to Menu.app
* Non-native applications: For non-native applications, the Canonical AppMenu protocol is supported in Menu.app via D-Bus
* Gtk applications: For legacy Gtk applications, the deprecated Gtk protocol is supported in Menu.app via D-Bus

### Chrome

Should just work.

### Firefox

Needs a patch upstream has been refusing to merge for years.

### References

https://github.com/helloSystem/docs/blob/main/developer/menu.md
