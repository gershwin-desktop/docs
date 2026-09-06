# Building Gershwin

Gershwin is built using [gershwin-build](https://github.com/gershwin-desktop/gershwin-build), which orchestrates building all components from source.

## Quick start

```console
git clone https://github.com/gershwin-desktop/gershwin-build.git
cd gershwin-build
./bootstrap.sh
./checkout.sh
make install
```

## Per-component builds

After building the core libraries once, individual components can be rebuilt independently:

```console
cd /Developer
make corelibs       # GNUstep base, gui, back, libobjc2
make workspace      # gershwin-workspace
make windowmanager  # gershwin-windowmanager
make components     # Menu, LoginWindow, etc.
make systempreferences
make terminal
make textedit
```

## Live ISOs

The various Live ISO creation scripts (FreeBSD, Debian, Arch) run `gershwin-build` as part of their build process. See `gershwin-desktop/` for the ISO definitions.

## Build targets

| Target | Description |
|---|---|
| `corelibs` | Core libraries and system infrastructure |
| `workspace` | File manager and desktop |
| `windowmanager` | Native X11 window manager |
| `components` | System components (Menu, LoginWindow, etc.) |
| `systempreferences` | System Preferences app |
| `terminal` | Terminal emulator |
| `textedit` | Text editor |

## Requirements

- Clang/LLVM toolchain
- GNUstep make (`tools-make`)
- X.org development headers
- Per-component dependencies documented in each repository
