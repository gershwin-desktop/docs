# Linux

Gershwin can be built and run on Linux. The supported distributions are the ones
with a package list in `Library/OSSupport/`: Debian, Devuan, Ubuntu, Arch Linux,
Artix, Manjaro, and Void.

## Building

Clone [gershwin-developer](https://github.com/gershwin-desktop/gershwin-developer)
and run the bootstrap and checkout scripts. These run as root:

```console
git clone https://github.com/gershwin-desktop/gershwin-developer.git /Developer
/Developer/Library/Scripts/bootstrap.sh
/Developer/Library/Scripts/checkout.sh
cd /Developer && make install
```

See [Building Gershwin](building.md) for the per-component build targets and the
`checkout.sh` environment variables.

## Requirements

- GNUstep
- X.org or XFree86
- A window manager (WindowManager for native Gershwin, or a compatible X11 WM)

## Known differences

- The `initgfx` graphics autoconfiguration script is FreeBSD-specific
- D-Bus integration may behave slightly differently than on BSD
- Init handling varies by distribution: systemd on Debian, Ubuntu and Arch;
  sysvinit on Devuan; runit on Void and Artix. LoginWindow ships a service for
  systemd, sysvinit and FreeBSD `rc.d` only, so on runit the session has to be
  started from `startx` or a display manager.
