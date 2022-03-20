# Arch Linux Laptop Setup

## ISO
- burn iso to usb drive
- boot from usb drive

## INSTALL

### Set up wifi
list devices
```iw dev```
check device up
```ip link show $DEVICE```
set device up
```ip link set $DEVICE up```
connection status
```iw $DEVICE link```
scan for wifi ssids
```iw $DEVICE scan```
set up ssid connection details
```wpa_passphrase $SSID >> /etc/wpa_supplicant.conf```
confirm
```cat /etc/wpa_supplicant.conf```
connect
```wpa_supplicant -B -D wext -i $DEVICE -c /etc/wpa_supplicant.conf```

### Checks
check for EFI enabled bios
```ls /sys/firmware/efi/efivars```
should return a list of files

### Attempt Mirrors Update
```
pacman -Sy
pacman -S reflector
reflector --latest 5 --download-timeout 60 --sort rate --save /etc/pacman.d/mirrorlist
```

### Drives
tools for checking current state
```lsblk```
```fdisk -l```
tools for setting partitions
```cfdisk /dev/$DISK```
```gdisk /dev/$DISK```
delete everything if you need to 
```dd if=/dev/zero of=/dev/$DISK bs=512 count=1```

for dualboot system using gdisk:
boot (EF00)
```
Command: n
ENTER
ENTER
+1G
EF00
```
swap (8200)
- should be size of ram
```
Command: n
ENTER
ENTER
+8G
8200
```
root (8304)
```
Command: n
ENTER
ENTER
+50G
8304
```
home (8302)
```
Command: n
ENTER
ENTER
+50G
8302
```
root 2
```
Command: n
ENTER
ENTER
+50G
8304
```
home 2
```
Command: n
ENTER
ENTER
+50G
8302
```
data (8300)
```
Command: N
ENTER
ENTER
ENTER
ENTER
```
### Format partitions
```
mkfs.fat -F32 /dev/sda1
mkswap /dev/sda2
mkfs.ext4 /dev/sda3
mkfs.ext4 /dev/sda4
mkfs.ext4 /dev/sda5
mkfs.ext4 /dev/sda6
mkfs.ext4 /dev/sda7
```
### mount partitions
```
swapon /dev/sda2
mount /dev/sda3 /mnt
mkdir /mnt/{boot,home}
mount /dev/sda1 /mnt/boot
mount /dev/sda4 /mnt/home
```
confirm with
```lsblk```

## Installation
Update the system clock
```timedatectl set-ntp true```
Install Arch packages
```pacstrap /mnt base base-devel openssh linux linux-firmware neovim```
Generate fstab file
```genfstab -U /mnt >> /mnt/etc/fstab```

## Configure
```arch-chroot /mnt```
uncomment en_US.UTF-8 UTF-8 in /etc/locale.gen
```locale-gen```
add the following to /etc/locale.conf
```
LANG=en_US.UTF-8
LANGUAGE=en_US
LC_ALL=C
```
add the following to /etc/vconsole.conf
```KEYMAP=us```
timezone
```timedatectl set-timezone MST```
