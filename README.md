# Manjaro custom ISO profile
This is a clone from the original [Manjaro iso-profiles repository](https://gitlab.manjaro.org/profiles-and-settings/iso-profiles).

The package **[manjaro-iso-profiles-base](https://manjaristas.org/branch_compare?q=manjaro-iso-profiles-base)** is built with this [PKGBUILD](https://gitlab.manjaro.org/packages/extra/manjaro-iso-profiles), which is not using the last commit (not sure why).

The idea of this repository is to create a custom Manjaro ISO with custom packages. The profile files are in the directory [manjaro/yuki](manjaro/yuki). 

## How to use this?

This repository must be used in a [Manjaro](https://manjaro.org/) distribution (I use in a virtual machine), and here is the [documentation to build custom ISO](https://wiki.manjaro.org/index.php?title=Build_Manjaro_ISOs_with_buildiso).

Basic steps, as **root**:

``` bash
# install dependencies
pacman install manjaro-tools-iso git 

# clone this repository
git clone https://github.com/yuki/manjaro-custom-iso-profile 

# move manjaro-default profile's directory
mv /usr/share/manjaro-tools/iso-profiles /usr/share/manjaro-tools/iso-profiles-old 

# link this repository to the real path
ln -s /root/manjaro-custom-iso-profile /usr/share/manjaro-tools/iso-profiles
```

After this, to build the ISO:

``` bash
buildiso -p yuki -k linux69 -f
```

* **-p**: profile to create the ISO
* **-k**: kernel to use
* **-f**: Build full ISO (extra=true). Otherwise, the size will be less.


## iso-profiles


### profile.conf

~~~
##########################################
###### use this file in the profile ######
##########################################

## use multilib packages; x86_64 only
# multilib="true"

## use extra packages as defined in pkglist to activate a full profile
# extra="false"

################ install ################

## default displaymanager: none
## supported; lightdm, sddm, gdm, lxdm, mdm
# displaymanager="none"

## Set to false to disable autologin in the livecd
# autologin="true"

## nonfree xorg drivers
# nonfree_mhwd="true"

## possible values: grub;systemd-boot
# efi_boot_loader="grub"

## configure calamares for netinstall
# netinstall="false"

## configure calamares for mhwd
# mhwd_used="true"

## configure calamares for oem
# oem_used="false"

## configure calamares to use chrootcfg instead of unpackfs; default: unpackfs
# chrootcfg="false"

## use geoip
# geoip="true"

## unset defaults to given values
## names must match systemd service names
# enable_systemd=('bluetooth' 'cronie' 'ModemManager' 'NetworkManager' 'org.cups.cupsd' 'tlp' 'tlp-sleep')
# disable_systemd=()

## unset defaults to given values
# addgroups="video,power,disk,storage,optical,network,lp,scanner,wheel"

## the same workgroup name if samba is used
# smb_workgroup="Manjaro"

## default system shell is bash
## '/etc/defaults/useradd': " "
## userShell              : "/bin/zsh"
## empty value will not be used
# user_shell=" "

## use calamares office installer
## supported: true,false
# office_installer="false"

## add strict snaps: strict_snaps="snapd core core18 gnome-3-28-1804 gtk-common-themes snap-store"
# strict_snaps=""

## add classic snaps: classic_snaps="code"
# classic_snaps=""

## choose the snap channel.
## supported:: stable,candidate,beta,edge
# snap_channel="candidate"

########## calamares preferences ##########
# See /etc/manjaro-tools/branding.desc.d for reference
# These settings will override settings from manjaro-tools.conf

## welcome style for calamares
## true="Welcome to the %1 installer."
## false="Welcome to the Calamares installer for %1." (default)
# welcomestyle=false

## welcome image scaled (productWelcome)
# welcomelogo=true

## size and expansion policy for Calamares
## supported: normal,fullscreen,noexpand
# windowexp=noexpand

## size of Calamares window, expressed as w,h
## supported: pixel (px) or font-units (em))
# windowsize="800px,520px"

## placement of Calamares window
## supported: center,free
# windowplacement="center"

## colors for text and background components:

## background of the sidebar
# sidebarbackground=#454948

## text color
# sidebartext=#efefef

## background of the selected step
# sidebartextselect=#4d915e

## text color of the selected step
# sidebartexthighlight=#1a1c1b

################# live-session #################

## unset defaults to given value
# hostname="manjaro"

## unset defaults to given value
# username="manjaro"

## unset defaults to given value
# password="manjaro"

## the login shell
## defaults to bash
# login_shell=/bin/bash

## unset defaults to given values
## names must match systemd service names
## services in enable_systemd array don't need to be listed here
# enable_systemd_live=('manjaro-live' 'mhwd-live' 'pacman-init' 'mirrors-live')
disable_systemd_live=('tlp' 'tlp-sleep')

~~~

### Packagelist tags

~~~
>systemd

>x86_64
>multilib

>nonfree_default
>nonfree_x86_64
>nonfree_multilib

>manjaro

>basic
>extra
~~~

### Packages-Root

* Contains root image packages
* ideally no xorg

### Packages-Desktop

* Contains the desktop image packages
* desktop environment packages go here

### Packages-Mhwd

* Contains the MHWD driver packages repo

### Packages-Live

* Contains packages you only want in live session but not installed on the target system with installer
* default files are in shared folder and can be symlinked or defined in a real file

### buildiso can be configured to use custom repos

* create a user-repos.conf

~~~
${profile_dir}/user-repos.conf
~~~

**Add only your repos to user-repos.conf!**

**Important**: Only online repos is allowed in the user-repos.conf. Buildiso will fail on file-based repos.


### Calamares
netgroups definitions go in [this](https://github.com/manjaro/calamares-netgroups) repo please
