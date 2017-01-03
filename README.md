# lxqt-common

## Overview

This repository comprises a number of supportive files used by various LXQt components.   

Among other these are graphics files, themes, desktop entry files according to the [XDG Desktop Menu Specification](https://www.freedesktop.org/wiki/Specifications/menu-spec/), template configuration files of various components like PCManFM-Qt or window manager Openbox as well as script `startlxqt` used to initialize LXQt sessions.

The LXQt logo was designed by @Caig and is licensed CC-BY-SA 3.0. LXQt theme "Plasma" is based on KDE Plasma Next theme by the KDE Visual Team.

## Installation

### Compiling/Packaging source code
#### Build dependencies:
* cmake
* lxqt-build-tools
* git (optional for translations)
#### Dependencies:
* hicolor-icon-theme
* x11-utils (*/bin/xmessage)
* perl-file-mimeinfo

Code configuration is handled by CMake. CMake variable `CMAKE_INSTALL_PREFIX` has to be set to `/usr` on most operating systems.

To build run `make`, to install `make install` which accepts variable `DESTDIR` as usual.   

### Binary packages

The library is provided by all major Linux distributions like Arch Linux, Debian, Fedora and openSUSE. Just use your package manager to search for string `lxqt-common`.

## Usage

#### LXQt specific configuration of window manager Openbox

Window manager Openbox is by default storing user settings in file `$XDG_CONFIG_HOME/openbox/rc.xml`, normally `/home/<user>/.config/openbox/rc.xml`. When Openbox is used as window manager of LXQt a file `$XDG_CONFIG_HOME/openbox/lxqt-rc.xml` is used instead. This allows for storing LXQt-specific settings while at the same time using different settings when Openbox is e. g. used in stand-alone window manager "only" sessions without any deskop environment. LXDE is using a custom configuration file `$XDG_CONFIG_HOME/openbox/lxde-rc.xml` the very same way.   
In order to keep backwards compatibility those configuration files are handled by LXQt as follows:
* `lxqt-common` ships a template file `$XDG_CONFIG_DIRS/openbox/lxqt-rc.xml`, normally `/etc/xdg/openbox/lxqt-rc.xml`.
* At the beginning of each LXQt session script `startlxqt` checks whether any of the files `rc.xml`, `lxde-rc.xml` or `lxqt-rc.xml` preexists in `$XDG_CONFIG_HOME/openbox/`.
* If neither of these files preexists `$XDG_CONFIG_DIRS/openbox/lxqt-rc.xml` is copied to `$XDG_CONFIG_HOME/openbox/lxqt-rc.xml`. If either `rc.xml` or `lxde-rc.xml` preexists in `$XDG_CONFIG_HOME/openbox/` it is copied to `$XDG_CONFIG_HOME/openbox/lxqt-rc.xml` as well.   
  Both times users are informed by binary `xmessage`.
* Either way this will result in file `$XDG_CONFIG_HOME/openbox/lxqt-rc.xml` being available which is from then on used within LXQt sessions.
