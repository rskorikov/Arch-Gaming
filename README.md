# Arch-Gaming
Step by step guide for optimizing Arch linux gaming performance
## Arch post-install config. Gaming and Multimedia
## The source is youtube video:
https://www.youtube.com/watch?v=5mn6xHCxTp4&t=2141s

## Install fonts:
```
sudo pacman -S noto-fonts ttf-bitstream-vera ttf-dejavu ttf-droid ttf-liberation ttf-roboto ttf-jetbrains-mono-nerd ttf-cascadia-code-nerd ttf-hack-nerd
```
## Install/update GPU drivers:
**AMD:**
```
sudo pacman -S mesa lib32-mesa mesa-utils mesa-vdpau lib32-mesa-vdpau lib32-vulkan-radeon vulkan-radeon glu lib32-glu xf86-video-amdgpu vulkan-icd-loader lib32-vulkan-icd-loader vulkan-tools
```
**Nvidia rtx 2000 and newer:**
```
sudo pacman -S nvidia-open nvidia-settings nvidia-utils lib32-nvidia-utils lib32-opencl-nvidia opencl-nvidia libvdpau libxnvctrl libva-nvidia-driver
```
**Vulkan:**
```
sudo pacman -S vulkan-icd-loader lib32-vulkan-icd-loader
```
## Install Steam and optimize PC performance for gaming:
**Install Steam:**
```
sudo pacman -S steam ttf-liberation lib32-systemd
```
**Disable mouse acceleration**
* Wayland (Gnome and Plasma):
*Just use system mouse settings to activate 'flat' profile.*
* X11 (WM-staff):
1. Find out your device id:
```
xinput
```
3. To activate the flat profile:
```
xinput set-prop "deviceid" "libinput Accel Profile Enabled" 0 1 0
```
4. Confirm the changes:
```
xinput list-props "deviceid"
```
5. Make it persistent by adding the option to the pointer section:
edit file: '/etc/X11/xorg.conf.d/40-libinput.conf' and add the block:
``` 
 Section "InputClass"
  Identifier "libinput pointer catchall"
  MatchIsPointer "on"
  MatchDevicePath "/dev/input/event*"
  Driver "libinput"
  Option "AccelProfile" "flat"
 EndSection
```
**Install MangoHud and Goverlay for viewing performance stats**
* Install MangoHud:
```
sudo pacman -S mangohud lib32-mangohud
```
*To enable mangohud set the environment variable: MANGOHUD=1 or just mangohud*
* Install Goverlay (GUI to configure MangoHud look & feel):
```
sudo pacman -S goverlay
```
* To enable MangoHud in Steam add the line to game launch options:
mangohud %command%

**OPTIONAL (Install 'linux-zen' kernel for better system responsiveness)**
```
sudo pacman -S linux-zen linux-zen-headers
```
* To add menu option to systemd-boot cd to: /boot/loader/entries/
and add the new entry-file: linux-zen.conf
```
title   Arch Linux (linux-zen)
linux   /vmlinuz-linux-zen
initrd  /initramfs-linux-zen.img
options root=PARTUUID=29b925ba-be98-40de-ae19-ff2b377893bd zswap.enabled=0 rw rootfstype=ext4'
```
*SAVE the file and exit. Simply reboot and enjoy (or not)*
* To add menu option to grub:
```
update grub config 'sudo grub-mkconfig -o /boot/grub/grub.cfg' and reboot.
```
Select Advanced Options in the boot menu and then select Zen Kernel.
It will be auto set as default for future use.

**Improve Steam game-stability:**
Create config-file: /etc/sysctl.d/80-gamecompatibility.conf
with content: 
```
vm.max_map_count = 2147483642
```
Apply the changes without reboot by running: 
```
sudo sysctl --system
```
## Install and configure gamemode for additional optimization:**
* Install gamemode:
```
sudo pacman -S gamemode lib32-gamemode
```
* Add current user (rs in my case) to group gamemode:
```
sudo usermod -aG gamemode rs 
```
* Create folder for gamemode config file (gamemode.ini): 
```
sudo mkdir /usr/share/gamemode/
```
* Download example gamemode.ini file from:
[the link](https://github.com/FeralInteractive/gamemode/tree/master/example)
* Move gamemode.ini to /usr/share/gamemode/ folder:
```
sudo mv gamemode.ini /usr/share/gamemode/
```
* Enable gamemode-daemon (gamemoded) to be able to use gamemode:
```
systemctl --user enable --now gamemoded.service
```
* Test gamemode:
```
gamemoded -t
```
* To actually use gamemode in Steam add the line to game launch options:
```
gamemoderun %command%
```
which can be combined with another launch options (mangohud)

#### Have a good gaming!
