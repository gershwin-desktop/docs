# Autostart

The **Autostart** mechanism provides an easy way to automatically start applications whenever the computer is started and a user logs into a graphical desktop session.

:::{note}
As of January 25, 2026 this still to be developed.
:::

* **System-wide autostart**: Place applications (or symlinks to applications) into the `/Applications/Autostart` folder to have them started for all users.
* **Per-user autostart**: Place applications (or symlinks to applications) into the `~/Applications/Autostart` folder (in your home folder) to have them started for the current user.

Gershwin automatically scans these locations and launches the applications upon session initialization.
