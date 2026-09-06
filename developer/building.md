# Building Gershwin

Gershwin is built using [gershwin-developer](https://github.com/gershwin-desktop/gershwin-developer), which orchestrates building all components from source.

## Quick start

Run these as root. `bootstrap.sh` installs the build dependencies for your OS,
`checkout.sh` clones the component repositories into `Library/Sources`, and
`make install` builds and installs the system domain.

```console
git clone https://github.com/gershwin-desktop/gershwin-developer.git /Developer
/Developer/Library/Scripts/bootstrap.sh
/Developer/Library/Scripts/checkout.sh
cd /Developer && make install
```

To remove a build installed this way, run `cd /Developer && make uninstall`.

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

The various Live ISO creation scripts run `gershwin-developer` as part of their
build process. The ISO definitions live in the
[gershwin-desktop](https://github.com/gershwin-desktop/gershwin-desktop)
repository, one directory per base OS under `targets/`. To add a new flavor, see
its `docs/ADDING-A-FLAVOR.md`.

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
| `eau-theme` | Eau theme |

## Controlling the checkout

`checkout.sh` reads three environment variables.

`SKIP_REPOS` takes a space- or comma-separated list of repository names to leave
alone. Use it when you want to supply a repository's sources yourself — symlink
your own working copy into `Library/Sources/` and the build picks it up:

```console
SKIP_REPOS="gershwin-workspace" /Developer/Library/Scripts/checkout.sh
```

`BRANCH` checks out the named branch in every repository that has one, falling
back to the default branch elsewhere. A partial rollout therefore works without
further ceremony, which makes it useful for testing a feature branch that spans
several repositories:

```console
BRANCH=dev /Developer/Library/Scripts/checkout.sh
```

`PINNED=0` builds against the upstream HEADs instead of the pinned commits. The
upstream GNUstep libraries are pinned by default so that the patches in
`Library/Patches/` keep applying; unset the pins to check whether a pin can move
forward. Gershwin's own repositories are never pinned.

## Requirements

- Clang/LLVM toolchain
- GNUstep make (`tools-make`)
- X.org development headers
- The packages listed in `Library/OSSupport/` for your OS, which `bootstrap.sh`
  installs for you
