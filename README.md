![header image](ply_header.svg "header image")

## Patchs
* this version of Plymouth ignores ESC when booting and DOES NOT SHOW the console.

## Build
1. install dependencies
```
# debian/ubuntu:
sudo apt install build-essential meson ninja-build pkg-config \
libdrm-dev libxcb-render0-dev libxcb-shm0-dev libx11-dev libxext-dev \
libpng-dev libjpeg-dev libfreetype6-dev libsystemd-dev git \
libgl1-mesa-dev libegl1-mesa-dev libwayland-dev \
libevdev-dev xsltproc libpango1.0-dev libgtk-3-dev gettext \
libudev-dev docbook-xsl
```
2. make build directory
```
mkdir build
cd build
```
3. run meson from build directory
```
# use Meson cross-file for cross-compilation to a different architecture
meson .. --prefix=/usr

# local docbook
meson .. -Dman=true -Ddocbook_xsl=/usr/share/xml/docbook/stylesheet/docbook-xsl/manpages/docbook.xsl --prefix=/usr
```
4. run ninja from build directory
```
ninja
```
5. install to system directory
```
DESTDIR=test/mysystemroot ninja install
```


## Overview
Plymouth is an application that runs very early in the boot process (even before the root filesystem is mounted!) that provides a graphical boot animation while the boot process happens in the background.

It is designed to work on systems with DRM modesetting drivers. The idea is that early on in the boot process the native mode for the computer is set, plymouth uses that mode, and that mode stays throughout the entire boot process up to and after X starts. Ideally, the goal is to get rid of all flicker during startup.

For systems that don't have DRM mode settings drivers, plymouth falls back to text mode (it can also use a legacy `/dev/fb` interface).

In either text or graphics mode, the boot messages are completely occluded.  After the root file system is mounted read-write, the messages are dumped to `/var/log/boot.log`. Also, the user can see the messages at any time during boot up by hitting the escape key.

## Installation
Plymouth isn't really designed to be built from source by end users. For it to work correctly, it needs integration with the distribution. Because it starts so early, it needs to be packed into the distribution's initial ram disk, and the distribution needs to poke plymouth to tell it how boot is progressing.

### Binary Files
plymouth ships with two binaries:
- `/sbin/plymouthd` and
- `/bin/plymouth`

The first one, `plymouthd`, does all the heavy lifting. It logs the session and shows the splash screen. The second one, `/bin/plymouth`, is the control interface to `plymouthd`.

It supports things like plymouth `show-splash`, or plymouth `ask-for-password`, which trigger the associated action in `plymouthd`.

Plymouth supports various "splash" themes which are analogous to screensavers, but happen at boot time. There are several sample themes shipped with plymouth, but most distributions that use plymouth ship something customized for their distribution.

## Current Efforts
Plymouth isn't done yet. It's still under active development, but is used in several popular distros already, including Fedora, Mandriva, Ubuntu and others.  See the distributions page for more information.

## Code of Conduct
As with other projects hosted on freedesktop.org, Plymouth follows its Code of Conduct, based on the Contributor Covenant[^1]. Please conduct yourself in a respectful and civilized manner when using the above mailing lists, bug trackers, etc:

### References
[^1]: [freedesktop.org Contributor Covenant Code of Conduct](https://www.freedesktop.org/wiki/CodeOfConduct)
