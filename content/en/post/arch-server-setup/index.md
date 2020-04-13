---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Arch Server Setup"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2020-02-22T23:35:19+01:00
lastmod: 2020-02-22T23:35:19+01:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

# Quellen

* Unicks video https://www.youtube.com/watch?v=OTrZcIG4gDE
* Reboot luks with remote password https://wiki.archlinux.org/index.php/Dm-crypt/Specialties#Remote_unlocking_of_the_root_(or_other)_partition
* Managing EFI Boot Loaders for Linux:
Dealing with Secure Boot http://www.rodsbooks.com/efi-bootloaders/secureboot.html


* LUKS2 Support in GRUB
  * https://savannah.gnu.org/bugs/?55093
  * 


## Delete old hdds and ssds

## Start

loadkeys de-latin1-nodeadkeys

## Start ssh server for copy paste!!!

```
passwd
systemctl start sshd.service
ssh root@ip-adress
```

## Create partition layout

1. create GPT partition tabel
2. Create EFI paritition with type efi (ef00) ~300M
3. Create partition for luks as lionux file system 8300

```
gdisk /dev/sda
```
gdisk flow:

```
root@archiso ~ # gdisk /dev/sda
GPT fdisk (gdisk) version 1.0.4

Partition table scan:
  MBR: not present
  BSD: not present
  APM: not present
  GPT: not present

Creating new GPT entries in memory.

Command (? for help): n
Partition number (1-128, default 1):
First sector (34-937703054, default = 2048) or {+-}size{KMGTP}:
Last sector (2048-937703054, default = 937703054) or {+-}size{KMGTP}: +500M
Current type is 'Linux filesystem'
Hex code or GUID (L to show codes, Enter = 8300): ef00
Changed type of partition to 'EFI System'

Command (? for help): n
Partition number (2-128, default 2):
First sector (34-937703054, default = 1026048) or {+-}size{KMGTP}:
Last sector (1026048-937703054, default = 937703054) or {+-}size{KMGTP}:
Current type is 'Linux filesystem'
Hex code or GUID (L to show codes, Enter = 8300):
Changed type of partition to 'Linux filesystem'

Command (? for help): w

Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
PARTITIONS!!

Do you want to proceed? (Y/N): Y
OK; writing new GUID partition table (GPT) to /dev/sda.
The operation has completed successfully.
```

Now we have our filesyste like so:

```
lsblk -o NAME,TYPE,STYPE
NAME   TYPE FSTYPE
loop0  loop squashfs
sda    disk
├─sda1 part 
└─sda2 part 
sdb    disk
├─sdb1 part vfat
└─sdb2 part btrfs
sdc    disk
├─sdc1 part vfat
└─sdc2 part ext3

```

## Create efi filesystem

```
mkfs.vfat -F 32 -n ESP /dev/sdj1
```

## Create encryption device

luks options: https://wiki.archlinux.org/index.php/Dm-crypt/Device_encryption#Encryption_options_with_dm-crypt

```
cryptsetup -v -c aws-xts-plain64 -s 512 -h sha256 -i 2000 --use-random -y --type luks1 luksFormat /dev/sdj2
```

## Open encrypted partition

```
cryptsetup luksOpen /dev/sdj2 cryptroot
```

## Format cryptroot

```
mkfs.btrfs /dev/mapper/cryptroot
```

## Mount cryptroot to create subvolumes

```
mount /dev/mapper/cryptroot /mnt

btrfs sub create /mnt/@
btrfs sub create /mnt/@home
btrfs sub create /mnt/@pkg
btrfs sub create /mnt/@snapshots

umount /mnt

# Creating mountpoints

mount -o subvol=@ /dev/mapper/cryptroot /mnt

# creat mountpoints

mkdir -p /mnt/home -p /home
mkdir -p /mnt/.snapshots
mkdir -p /mnt/var/cache/pacman/pkg

# mount subvolumes

mount -o subvol=@home /dev/mapper/cryptroot /mnt/home
mount -o subvol=@pkg /dev/mapper/cryptroot /mnt/var/cache/pacman/pkg
mount -o subvol=@snapshots /dev/mapper/cryptroot /mnt/.snapshots

# mount efi partition

mkdir -p /mnt/boot/efi
mount /dev/sdj1 /mnt/boot/efi
```

final system:

```
root@archiso / # df -Th
Filesystem            Type      Size  Used Avail Use% Mounted on
dev                   devtmpfs   16G     0   16G   0% /dev
run                   tmpfs      16G  103M   16G   1% /run
/dev/sdc1             vfat       24G  647M   23G   3% /run/archiso/bootmnt
cowspace              tmpfs     256M  348K  256M   1% /run/archiso/cowspace
/dev/loop0            squashfs  532M  532M     0 100% /run/archiso/sfs/airootfs
airootfs              overlay   256M  348K  256M   1% /
tmpfs                 tmpfs      16G     0   16G   0% /dev/shm
tmpfs                 tmpfs      16G     0   16G   0% /sys/fs/cgroup
tmpfs                 tmpfs      16G     0   16G   0% /tmp
tmpfs                 tmpfs      16G  1.9M   16G   1% /etc/pacman.d/gnupg
tmpfs                 tmpfs     3.2G     0  3.2G   0% /run/user/0
/dev/mapper/cryptroot btrfs     447G  3.5M  447G   1% /mnt
/dev/mapper/cryptroot btrfs     447G  3.5M  447G   1% /mnt/home
/dev/mapper/cryptroot btrfs     447G  3.5M  447G   1% /mnt/var/cache/pacman/pkg
/dev/mapper/cryptroot btrfs     447G  3.5M  447G   1% /mnt/.snapshots
/dev/sda1             vfat      500M  4.0K  500M   1% /mnt/boot/efi

```

# Install arch linux

```
pacstrap /mnt base base-devel linux linux-firmware grub btrfs-progs efibootmgr dosfstools bash-completion vim cryptsetup openssh docker docker-compose sudo htop rsync
```

# Generate fstab with UUIDs

```
genfstab -U /mnt >> /mnt/etc/fstab
```

# Arch Install

```
arch-chroot /mnt

# hostname
echo "arch-server" > /etc/hostname

# time zone
ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime

```

## localization
```

# Uncomment your locale (LANG=en_US.UTF-8)
vim /etc/locale.gen
locale-gen

echo LANG=en_US.UTF-8 > /etc/locale.conf

# persist keyboard layout
echo KEYMAP=de-latin1-nodeadkeys > /etc/vconsole.conf

# hardware clocl

hwclock --systohc

```

## set root password

```
passwd
```

## add user
```
useradd -m -g users -G wheel -s /bin/bash andrej
passwd andrej
```

## Add user to sudoers

Uncomment suders file and allow wheel to use sudo
```
visudo
```
```
...
%wheel ALL=(ALL) ALL
...
```

### add ssh keys

```
mkdir -p /home/andrej/.ssh/
curl https://github.com/ajfriesen.keys >> /home/andrej/.ssh/authorized_keys
chown -R andrej: /home/andrej/ #fix rights since we are going with root here
```

# configure static ip

https://wiki.archlinux.org/index.php/Systemd-networkd#Wired_adapter_using_a_static_IP

```
vim /etc/systemd/network/20-wired.network
```

```
[Match]
Name=enp0s25

[Network]
Address=192.168.178.3/24
Gateway=192.168.178.1
DNS=192.168.178.1
DNS=8.8.8.8
```

# enable ssh

```
systemctl enable sshd.service
```

# enable network

```
systemctl enable systemd-networkd.service
```

# enable dns

```
systemctl enable systemd-networkd.service
```

# dns
https://wiki.archlinux.org/index.php/Systemd-resolved
systemctl start systemd-resolved.service

# User with sudo




# Todo

openssh deny
fail2ban
mirrorliste
https://wiki.archlinux.org/index.php/Systemd-timesyncd 

# configure grub to start with encrypted device

## get uuid of /dev/sdj2

```
[root@archiso /]# blkid
/dev/sdb1: UUID="F246-15B7" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="eea7e967-1d67-4398-a0d5-a4298ccc8765"
/dev/sdb2: UUID="2a49320d-9fc3-475b-979b-664f7134f076" UUID_SUB="3fba394e-8ebc-4b92-8593-19d43d18c889" BLOCK_SIZE="4096" TYPE="btrfs" PARTUUID="c7637ab2-b337-49e7-84e4-6102ab10e978"
/dev/sda1: LABEL_FATBOOT="ESP" LABEL="ESP" UUID="2374-1A2E" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="EFI System" PARTUUID="9e3a9100-4755-46cb-8630-8eac96073d1f"
/dev/sda2: UUID="d4a440cb-6103-499d-896a-91825e6567e2" TYPE="crypto_LUKS" PARTLABEL="Linux filesystem" PARTUUID="6c50c260-8ee7-42ec-98fc-be3716a77ccc"
/dev/sdc1: LABEL="ARCH_202002" UUID="72D2-6B05" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="0f9c2f31-01"
/dev/sdc2: LABEL="persistence" UUID="5966449c-7cba-0b48-ac8f-c3377ffd9117" SEC_TYPE="ext2" BLOCK_SIZE="4096" TYPE="ext3" PARTUUID="0f9c2f31-02"
/dev/loop0: TYPE="squashfs"
/dev/mapper/cryptroot: UUID="9075c852-f372-4ff4-8f21-1e6c38f2f567" UUID_SUB="45e9a92e-cebb-4386-8394-a307ba93042a" BLOCK_SIZE="4096" TYPE="btrfs"
```

vim /etc/default/grub

```
GRUB_CMD_LINE_LINUX="cryptdevice=UUID=d4a440cb-6103-499d-896a-91825e6567e2:cryptroot"
GRUB_ENABLE_CRYPTODISK=y
```

## Install grub
* https://wiki.archlinux.org/index.php/GRUB#UEFI_systems
```
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch-crypt
```

## Generate main config file for grub
```
grub-mkconfig -o /boot/grub/grub.cfg
```

## Create key for initramfs

```
dd bs=512 count=4 if=/dev/random of=/crypto_keyfile.bin iflag=fullblock
chmod 600 crypto_keyfile.bin
chmod 600 /boot/initramfs-linux-*
cryptsetup luksAddKey /dev/sda2 /crypto_keyfile.bin
```

## optional trick

since on boot you only have a us layout you could add your lets say german password with special german characters with the us layout
That way you can type the same key (physically on the keyboard) and can use the german and us version

```
loadkeys us
cryptsetup addKey
```

## Configure initram disk

```
vim /etc/mkinitcpio.conf
```

Add these lines

```
...
FILES=(/crypto_keyfile.bin)
...
BINARIES=(/usr/bin/btrfs)
...
HOOKS=(base udev autodetect modconf block keyboard keymap encrypt filesystems)
```

mkinitcpio -p linux

## efi
```
efibootmgr
```

exit
umount -R /mnt
cryptsetup luksClose cryptroot
reboot




# HASSIO

pacman -S apparmor avahi ca-certificates curl dbus jq socat


# Raid Setup

p1right UUID=54ad3028-f100-4391-8c3b-89ff3afd8c93 /home/andrej/keyfile/raidkey
p4left  UUID=2ffe7058-3b1b-4aea-aa46-3639015691e9 /home/andrej/keyfile/raidkey
p3left  UUID=d0335563-b766-419e-bb47-ecf745d131b0 /home/andrej/keyfile/raidkey
p2left  UUID=df9a15f4-0400-49f0-a4b0-dbff596c7f6b /home/andrej/keyfile/raidkey
p1left  UUID=addfb22b-ab33-4f0f-9817-2e7e556dff76 /home/andrej/keyfile/raidkey
p2right UUID=56759536-218d-49ca-be10-7efd4ef39a97 /home/andrej/keyfile/raidkey


| storage | HDD Variant      | Seriel number   | UUID                                 | Position in NAS |
| ------- | ---------------- | --------------- | ------------------------------------ | --------------- |
| 3.7T    | WDC WD40EFRX-68W | WD-WCC4E0906921 | addfb22b-ab33-4f0f-9817-2e7e556dff76 | P1 LINKS        |
| 3.7T    | WDC WD40EFRX-68W | WD-WCC4E0846246 | 2ffe7058-3b1b-4aea-aa46-3639015691e9 | P4 LINKS        |
| 3.7T    | WDC WD40EFRX-68W | WD-WCC4E0870141 | d0335563-b766-419e-bb47-ecf745d131b0 | P3 LINKS        |
| 3.7T    | WDC WD40EFRX-68W | WD-WCC4E0881674 | df9a15f4-0400-49f0-a4b0-dbff596c7f6b | P2 LINKS        |
| 1.4T    | WDC WD15EARS-00Z | WD-WMAVU2558882 |                                      | P4 RECHTS       |
| 2.7T    | WDC WD30EFRX-68A | WD-WMC1T4089646 |                                      | P3 RECHTS       |
| 3.7T    | WDC WD40EFRX-68N | WD-WCC7K7LRH5V0 | 56759536-218d-49ca-be10-7efd4ef39a97 | P2 RECHTS       |
| 3.7T    | WDC WD40EFRX-68W | WD-WCC4E1623938 | 54ad3028-f100-4391-8c3b-89ff3afd8c93 | P1 RECHTS       |


ID 258 gen 1693 top level 5 path @media
ID 272 gen 245 top level 5 path @audio
ID 274 gen 825 top level 5 path @backup
 

 

UUID=8a50f68b-65a3-4959-a4e7-309bf497d104       /home/andrej/raidpool/media     btrfs           rw,compress=zstd,subvolid=258,subvol=@media   0 0
UUID=8a50f68b-65a3-4959-a4e7-309bf497d104       /home/andrej/raidpool/audio     btrfs           rw,compress=zstd,subvolid=258,subvol=@audio   0 0
UUID=8a50f68b-65a3-4959-a4e7-309bf497d104       /home/andrej/raidpool/backup     btrfs           rw,compress=zstd,subvolid=258,subvol=@backup   0 0


# RAID Setup


```
sda      3.7T             WDC WD40EFRX-68W WD-WCC4E0906921 P1 LINKS
sdb      3.7T             WDC WD40EFRX-68W WD-WCC4E0846246 P4 LINKS
sdc      3.7T             WDC WD40EFRX-68W WD-WCC4E0870141 P3 LINKS
sdd      3.7T             WDC WD40EFRX-68W WD-WCC4E0881674 P2 LINKS

sde      1.4T crypto_LUKS WDC WD15EARS-00Z WD-WMAVU2558882 P4 RECHTS
sdf      2.7T             WDC WD30EFRX-68A WD-WMC1T4089646 P3 RECHTS
sdg      3.7T             WDC WD40EFRX-68N WD-WCC7K7LRH5V0 P2 RECHTS
sdh      3.7T             WDC WD40EFRX-68W WD-WCC4E1623938 P1 RECHTS
```

```
sudo cryptsetup -v --type luks2 --cipher aes-xts-plain64 --key-size 256 --hash sha256 --iter-time 2000 --use-urandom --verify-passphrase luksFormat /dev/sdg
```


sudo cryptsetup luksAddKey /dev/sdg /home/andrej/keyfile/raidkey

sudo cryptsetup open /dev/sdg p3right --key-file keyfile/raidkey



https://carfax.org.uk/btrfs-usage/index.html?utm_content=buffer8fb32&utm_medium=social&utm_source=facebook.com&utm_campaign=buffer