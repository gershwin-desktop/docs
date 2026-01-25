# Raspberry Pi

Gershwin is known to build and run very well on the Raspberry Pi 5/500. It builds in acceptable time and is usable well on the Raspberry Pi 4. It might build (untested) and is usable for light tasks on Raspberry Pi 3 and Raspberry Pi Zero 2 (the latter has only 256 MB of RAM).

## FreeBSD

FreeBSD 15 seemingly doesn't have good support for Raspberry Pi 5/500 yet (to be verified). Hence we recommend Linux (see below) for these until they are supported better by FreeBSD.

Known to build on FreeBSD 15 on Raspberry Pi 4. The resulting system can also run on Raspberry Pi 3 and Raspberry Pi Zero 2.

## Raspberry Pi OS (Debian)

Official Raspberry Pi OS image. If you use the desktop version, then LoginWindow will give you the option to log into the Raspberry Pi OS desktop or into the Gershwin desktop.

## Devuan

https://arm-files.devuan.org/RaspberryPi%20Latest%20Builds/, file `rpi-5-devuan-excalibur-*-arm64-ext4-*.zip`

> [!CAUTION]
> As of January 25, 2026 this in known to be broken.

SSH is preconfigured and enabled on the Devuan images.

```
ssh devuan@devuanpi.local # Password: devuan
```

## Instructions

Using the Raspberry Pi Imager tool, flash the OS image file to a microSD card (it creates the partitions automatically).

The rest is done via SSH, which is preconfigured on the Devuan image.

Then follow https://github.com/gershwin-desktop/gershwin-build standard procedure:

```
#  Get the rest of the requirements for building
git clone https://github.com/gershwin-desktop/gershwin-build.git && cd gershwin-build
sudo ./bootstrap.sh
./checkout.sh
# Build and install Gershwin from sources
sudo make install
```

Finally, run `/System/Library/Scripts/SystemPrepare.sh`. It will configure the system to boot straight into Gershwin Desktop. Reboot the system when it is done.
