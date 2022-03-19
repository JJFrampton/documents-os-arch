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
root #2
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
home 2
```
Command: n
ENTER
ENTER
+50G
8302
```
data
```
Command: N
ENTER
ENTER
ENTER
ENTER
```
