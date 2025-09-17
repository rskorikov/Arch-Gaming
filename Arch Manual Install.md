# Arch Manual Install (systemd-boot).

## This is the Guide for installing Archlinux manually with systemd-boot as a boot-loader.
### Prerequisites:
* Select the console keyboard-layout:
```
localectl list-keymaps # shows the list of all layouts.
loadkeys us # choose any layout that suits your needs.
```
* Set console font
```
setfont ter-132b # set biggest available font
setfont -d # set double font size
setfont # loads the default font
```
* Syncronize (update) system time and date with network time protocol (ntp):
```
timedatectl set-ntp true
```

* Test if internet connection is available:
```
ping www.archlinux.org
```
*In case of wireless network use* **iwctl** *command and follow instructions (interactive prompt).*

### Partition the disks:
1. Use **lsblk** command to see the existing disks and partitions structure on your system and to choose the place for new installation:
```
lsblk
```
*Disks are assigned to a block device such as /dev/sda, /dev/nvme0n1*

2. Create partitions with GPT label:
Arch Linux installation requires at least 2 partitions to be operational:
- EFI System partition (at least 512M size)
- Partition for root directory (/)
- (Optional) swap partition.
Use cfdisk for partitioning:
```
cfdisk /dev/disk_to_be_partitioned (sda, sdb, nvme0n1...)
```
3. Formatting partitions:
Format each created partition with appropriate file system.
- EFI partition: 
```
mkfs.fat -F32 /dev/efi_system_partition # sda1 as an example
```
- Root partition: 
```
mkfs.ext4 /dev/root_partition # sda2 as an example
```
- (Optional) swap partition: 
```
mkswap /dev/swap_partition # sda3 as an example
```

4. Mounting the file systems:
- Create mount point for EFI partition (EFI should be mounted to /mnt/boot/)
```
mkdir /mnt/boot/
```
- Mount root partition to /mnt
```
mount /dev/sda2 /mnt
```
- Mount EFI partition to /mnt/boot accordingly
```
mount /dev/sda1 /mnt/boot
```
- If you created a swap volume, enable it with swapon
```
swapon /dev/sda3
```

### Install Arch Linux base packages:
1. Install base system using **pacstrap** command.
```
pacstrap /mnt base linux linux-firmware sof-firmware nano bash-completion git base-devel
```
2. Generate fstab file to save all mount points.
```
genfstab -U /mnt >> /mnt/etc/fstab
```
3. Configure the installed system using **chroot** and entering installation directory
```
arch-chroot /mnt
```
- Set local timezone:
```
ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime
hwclock --systohc
```
4. Generate and enable system locale:
  
Using text editor open the file **locale.gen**
```
nano /etc/locale.gen
```
To enable Russian locale find and uncomment the line:
```
#ru_RU.UTF-8
```
then exec the command:
```
locale-gen
```
to generate the locale.
Then add the generated locale to /etc/locale.conf file.
```
echo 'LANG=ru_RU.UTF-8' >> /etc/locale.conf
```
5. Give your system a hostname:
```
echo 'arch-btw' >> /etc/hostname
```
6. Edit hosts file
```
nano etc/hosts
```
Add the line at the end of the file
```
127.0.0.1   arch-btw.localdomain  arch-btw
```
7. Create password for your root
```
passwd
```












