# Linux

Gershwin can be built and run on Linux. The primary supported distributions are Debian, Devuan, Arch Linux, and Artix.

## Building

Use [gershwin-build](https://github.com/gershwin-desktop/gershwin-build) for building from source.

For manual development setup, clone `gershwin-developer` and run the bootstrap and checkout scripts:

```console
git clone https://github.com/gershwin-desktop/gershwin-developer.git /Developer
/Developer/Library/Scripts/bootstrap.sh
/Developer/Library/Scripts/checkout.sh
cd /Developer && sudo make install
```

## Requirements

- GNUstep
- X.org or XFree86
- A window manager (WindowManager for native Gershwin, or a compatible X11 WM)

## Known differences

- The `initgfx` graphics autoconfiguration script is FreeBSD-specific
- D-Bus integration may behave slightly differently than on BSD
- Some system services use systemd or OpenRC depending on the distribution
