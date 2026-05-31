# Workspace

__Workspace__ is Gershwin's application for managing files, folders, and applications. It is the core of the Gershwin desktop experience.

Workspace offers several viewing modes:
* **Column view**: A classical multi-column browser view.
* **List view**: A detailed list of files and their properties.
* **Icon view**: Files shown as icons, with support for thumbnails.

## Core Features
* **Desktop and Dock**: Workspace provides an optional desktop with a dock for keeping track of favorites and running applications.
* **Inspector**: A powerful contents inspector with image previews, extensible through plug-ins.
* **Volume Management**: Easily check and unmount volumes.
* **Advanced Search**: Optional database-backed search and live folders.
* **NSWorkspace implementation**: Offers services to other applications for opening files and performing file operations.

## Launching Applications
Applications are typically distributed as bundles and can be launched by double-clicking them in Workspace.

## Networking

Network devices, such as SFTP servers, are automatically detected and can be accessed from Workspace from Go -> Network.

### Windows SFTP server

To make a Windows 11 computer show up there, you need to:
* Install the OpenSSH Server in Windows 11. To do so,
run PowerShell as Administrator, then run:
```
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Start-Service sshd
Set-Service -Name sshd -StartupType Automatic
```
* Optionally if you do not have a password set for your Windows user, set `PermitEmptyPasswords yes` in `%programdata%\ssh\sshd_config` which is `C:\ProgramData\ssh\sshd_config` usually (need to do as an Administrator)
* Install Bonjour for Windows. It comes with iTunes for Windows but can also be installed standalone from [here](https://github.com/manfred-mueller/Bonjour-Installer/releases) (NOTE: Potentially https://support.apple.com/en-us/106380 also works instead; still to be tested.)
* To advertise the sshd server on the network, run `dns-sd -R "MyWindowsBox" _sftp-ssh._tcp local 22` on the Windows machine. This Windows Service does it automatically: https://github.com/probonopd/BonjourServiceAdvertiser
* Disable the Windows firewall (TODO: Document how to just enable the needed ports)
You should now be able to see and access the Windows 11 SFTP server from Gershwin.
