# sway-gnome

[![License](http://img.shields.io/badge/license-MIT-blue.svg?style=flat)](http://choosealicense.com/licenses/mit/)

---

Allows you to use [Sway](https://github.com/swaywm/sway), a tiling window manager, with GNOME 3 Session
infrastructure on Linux distributions which have GNOME >= 3.34.

This is a fork of the original project. Changes in this fork include:

- A PKGBUILD for Arch Linux
- Ability to manually start sway
- Uses the gnome config provided by the original project (was disabled in the fork I forked)

## Work in progress

This repository is currently work-in-progress. Right now, the Sway session is
started by GDM (but it can also be manually started), modelled after sway's [systemd integration wiki page](https://github.com/swaywm/sway/wiki/Systemd-integration).

Some further unit files are included to launch as many GNOME deamons in the background as possible in a non-gnome-shell wayland session.
Not all daemons are enabled and further work is necessary for full-compatibility!

## What this enables

-   Keybindings for controlling brightness, Play/Pause, Next/Previous Track, Mute, Volume Up/Down. You can customize [sway/config.d/gnome](./sway/config.d/gnome) after installation
-   Desktop integration for Flatpak and Snap
-   Idle management / Screen Lock
-   Automatic screen adjustment at sunrise / sunset
-   Privilege management
-   Keyring integration
-   Dynamic display configuration
-   Accessibility Settings
-   Color Management Settings
-   Date & Time Settings
-   Keyboard Settings
-   Power Management Settings
-   Printer Notifications
-   Enabling and Disabling Wireless Devidces (rfkill)
-   Screensaver Settings
-   Handle Sharing music, pictures and videos on the local network
-   Remote Login settings
-   Smartcard handling
-   Sound settings
-   Wacom tablet handling
-   WWAN handling for modems / SIM Cards
-   Display Server settings
-   Clipboard

## Usage

Install the system files using:

```shell
meson builddir && cd builddir
meson compile
meson install
```

Then add this line to your sway config:

```sway
include /etc/sway/config.d/*
```

In your login manager `Sway (systemd)` should be startable as a new session. Alternatively, you can manually start sway using:

```shell
sway-user-session
```

## Tips and tricks

### Unlock the keyring on login

Make sure the keyring password for the default keyring is the same as your user password.

Then edit `/etc/pam.d/login` and add the **bold** lines:

```pam
#%PAM-1.0

auth       required     pam_securetty.so
auth       requisite    pam_nologin.so
auth       include      system-local-login
**auth     optional     pam_gnome_keyring.so**
account    include      system-local-login
session    include      system-local-login
**session  optional     pam_gnome_keyring.so auto_start**
```

You can also add this line to `/etc/pam.d/passwd` to change the login password when user password is changed:

```pam
#%PAM-1.0

#password   required    pam_cracklib.so difok=2 minlen=8 dcredit=2 ocredit=2 retry=3
#password   required    pam_unix.so sha512 shadow use_authtok
password    required    pam_unix.so sha512 shadow nullok
**password  optional    pam_gnome_keyring.so**
```

### Enable Xorg apps

To enable Xorg apps, install Xwayland:

```shell
pacman -S xorg-xwayland
```

## Dependencies

You need to manually install these dependencies first:

-   brightnessctl - support keybindings for screen brightness control
-   network-manager-gnome - Network Manager control applet
-   pulseaudio-utils - support keybindings for volume control
-   playerctl - support binding media keys
-   xdg-desktop-portal - desktop integration for Flatpak and Snap
-   swayidle - for idle management
-   swaylock - for screen lock
-   redshift - for automatic screen dimming
-   policykit-1-gnome - privilege management
-   mako - lightweight Wayland
-   [kanshi](https://github.com/emersion/kanshi) - Dynamic display configuration for Wayland
-   gnome-keyring - manage SSH keys, PKCS11 and other secrets
-   gnome-session-bin - the gnome session binary itself
-   gnome-settings-daemon-common - provides GNOME settings services.

## Installation

While this project can be installed with a simple `sudo make install` after fetching the Git repo,
more effort may be required to install the dependencies, especially if they aren't packaged for your
Linux distribution. Distro-specific extensions are maintained on the wiki.

-   [Installing on Ubuntu 19.10](https://github.com/RobinBoers/sway-gnome/wiki/Installation#install-on-ubuntu-1910)
-   [Installing on Arch Linux](https://github.com/RobinBoers/sway-gnome/wiki/Installation#install-on-arch-linux)

## Related Projects

-   [sway-services](https://github.com/xdbob/sway-services) provides a minimal sway / systemd integration with no GNOME services
