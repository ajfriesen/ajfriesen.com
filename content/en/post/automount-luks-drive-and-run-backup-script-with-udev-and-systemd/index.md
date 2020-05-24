---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Automount LUKS drive and run a backup script with udev and systemd"
subtitle: ""
summary: ""
authors: []
tags: [systemd, Arch Linux, udev, LUKS, linux]
categories: []
date: 2020-05-24T23:22:43+02:00
lastmod: 2020-05-24T23:22:43+02:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "HDD toaster, no food was wasted"
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

I have on of these handy dandy little HDD toasters.
I just wanted to create some cold backups.
As everybody, I never used that thing.

Why?
I to lazy to do that, since it was always a manuall process.
That changes now with automount a LUKS drive with udev and systemd to back up my data.

<!--more-->

## The goal

1. Flip the power switch on that toaster (the HDDs is always inside one of the slots)
2. Automatlicy decrypt the LUKS device
3. Mount the btrfs filesystem
4. Copy some data with a backup script
5. Unmount after the backup has finished
6. Encrypt again
7. Maybe some sort of notification so that I know, that I safly swich off the hdd toaster (Not done yet)

The drive already is encrypted with LUKS2 and has a btrfs filesystem.
This will not be covered here.

## Create the udev rule for the specifc device

The udev documentation is not that pleasant to read and somehow I feel the information is gathered all over the palce.
So I started on the arch wik about udev [^1].
This got me to writing udev rules [^3] articel which is good for understanding the conecpt.
Please be carefull since some information on that article is outdated.
An example is that in thies article he is using `udevinfo` to gather information about the device.
This tool is no longer available at least in arch linux and I think the succsessor is `udevadm`.

I also found a german blog post [^2] about some simliar usecase.

I tried to gather some specific information for my HDD but the information I got with the follwing output were not working with udev.
I am still not sure why.

```bash
udevadm info -q all -n /dev/sdk
P: /devices/pci0000:00/0000:00:01.1/0000:02:00.0/usb4/4-2/4-2.3/4-2.3:1.0/host7/target7:0:0/7:0:0:0/block/sdk
N: sdk
L: 0
S: disk/by-id/wwn-0x5000c500a1f3eab8
S: disk/by-id/ata-ST8000AS0002-1NA17Z_Z840YC3T
S: disk/by-path/pci-0000:02:00.0-usb-0:2.3:1.0-scsi-0:0:0:0
S: disk/by-uuid/02d4c98c-f174-49ee-a686-a813b5695bf0
E: DEVPATH=/devices/pci0000:00/0000:00:01.1/0000:02:00.0/usb4/4-2/4-2.3/4-2.3:1.0/host7/target7:0:0/7:0:0:0/block/sdk
E: DEVNAME=/dev/sdk
E: DEVTYPE=disk
E: MAJOR=8
E: MINOR=160
E: SUBSYSTEM=block
E: USEC_INITIALIZED=113992668902
E: ID_ATA=1
E: ID_TYPE=disk
E: ID_BUS=ata
E: ID_MODEL=ST8000AS0002-1NA17Z
E: ID_MODEL_ENC=ST8000AS0002-1NA17Z\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
E: ID_REVISION=AR17
E: ID_SERIAL=ST8000AS0002-1NA17Z_Z840YC3T
E: ID_SERIAL_SHORT=Z840YC3T
E: ID_ATA_WRITE_CACHE=1
E: ID_ATA_WRITE_CACHE_ENABLED=1
E: ID_ATA_FEATURE_SET_HPA=1
E: ID_ATA_FEATURE_SET_HPA_ENABLED=1
E: ID_ATA_FEATURE_SET_PM=1
E: ID_ATA_FEATURE_SET_PM_ENABLED=1
E: ID_ATA_FEATURE_SET_SECURITY=1
E: ID_ATA_FEATURE_SET_SECURITY_ENABLED=0
E: ID_ATA_FEATURE_SET_SECURITY_ERASE_UNIT_MIN=66506
E: ID_ATA_FEATURE_SET_SECURITY_ENHANCED_ERASE_UNIT_MIN=66506
E: ID_ATA_FEATURE_SET_SMART=1
E: ID_ATA_FEATURE_SET_SMART_ENABLED=1
E: ID_ATA_FEATURE_SET_PUIS=1
E: ID_ATA_FEATURE_SET_PUIS_ENABLED=0
E: ID_ATA_DOWNLOAD_MICROCODE=1
E: ID_ATA_SATA=1
E: ID_ATA_SATA_SIGNAL_RATE_GEN2=1
E: ID_ATA_SATA_SIGNAL_RATE_GEN1=1
E: ID_ATA_ROTATION_RATE_RPM=5980
E: ID_WWN=0x5000c500a1f3eab8
E: ID_WWN_WITH_EXTENSION=0x5000c500a1f3eab8
E: ID_PATH=pci-0000:02:00.0-usb-0:2.3:1.0-scsi-0:0:0:0
E: ID_PATH_TAG=pci-0000_02_00_0-usb-0_2_3_1_0-scsi-0_0_0_0
E: ID_FS_VERSION=2
E: ID_FS_UUID=02d4c98c-f174-49ee-a686-a813b5695bf0
E: ID_FS_UUID_ENC=02d4c98c-f174-49ee-a686-a813b5695bf0
E: ID_FS_TYPE=crypto_LUKS
E: ID_FS_USAGE=crypto
E: DEVLINKS=/dev/disk/by-id/wwn-0x5000c500a1f3eab8 /dev/disk/by-id/ata-ST8000AS0002-1NA17Z_Z840YC3T /dev/disk/by-path/pci-0000:02:00.0-usb-0:2.3:1.0-scsi-0:0:0:0 /dev/disk/by-uuid/02d4c98c-f174-49ee-a686-a813b5695bf0
E: TAGS=:systemd:
```

Then I found this command:

```bash
udevadm info -a -p $(udevadm info -q path -n /dev/sdk)

Udevadm info starts with the device specified by the devpath and then
walks up the chain of parent devices. It prints for every device
found, all possible attributes in the udev rules key format.
A rule to match, can be composed by the attributes of the device
and the attributes from one single parent device.

  looking at device '/devices/pci0000:00/0000:00:01.1/0000:02:00.0/usb4/4-2/4-2.3/4-2.3:1.0/host7/target7:0:0/7:0:0:0/block/sdk':
    KERNEL=="sdk"
    SUBSYSTEM=="block"
    DRIVER==""
    ATTR{events_poll_msecs}=="-1"
    ATTR{hidden}=="0"
    ATTR{events}==""
    ATTR{removable}=="0"
    ATTR{size}=="15628053168"
    ATTR{discard_alignment}=="0"
    ATTR{capability}=="50"
    ATTR{alignment_offset}=="0"
    ATTR{ro}=="0"
    ATTR{inflight}=="       0        0"
    ATTR{stat}=="      23        0      161       69        0        0        0        0        0       44       54        0        0        0        0        0        0"
    ATTR{events_async}==""
    ATTR{ext_range}=="256"
    ATTR{range}=="16"

  looking at parent device '/devices/pci0000:00/0000:00:01.1/0000:02:00.0/usb4/4-2/4-2.3/4-2.3:1.0/host7/target7:0:0/7:0:0:0':
    KERNELS=="7:0:0:0"
    SUBSYSTEMS=="scsi"
    DRIVERS=="sd"
    ATTRS{iodone_cnt}=="0x3d"
    ATTRS{timeout}=="30"
    ATTRS{wwid}=="t10.ST8000AS0002-1NA17Z     00A123456884        0000"
    ATTRS{iocounterbits}=="32"
    ATTRS{evt_soft_threshold_reached}=="0"
    ATTRS{evt_mode_parameter_change_reported}=="0"
    ATTRS{dh_state}=="detached"
    ATTRS{queue_type}=="simple"
    ATTRS{type}=="0"
    ATTRS{scsi_level}=="7"
    ATTRS{rev}=="0107"
    ATTRS{iorequest_cnt}=="0x3d"
    ATTRS{vpd_pg80}==""
    ATTRS{evt_inquiry_change_reported}=="0"
    ATTRS{device_blocked}=="0"
    ATTRS{vpd_pg0}==""
    ATTRS{vendor}=="ST8000AS"
    ATTRS{inquiry}==""
    ATTRS{evt_lun_change_reported}=="0"
    ATTRS{state}=="running"
    ATTRS{blacklist}==""
    ATTRS{ioerr_cnt}=="0x2"
    ATTRS{device_busy}=="0"
    ATTRS{eh_timeout}=="10"
    ATTRS{evt_capacity_change_reported}=="0"
    ATTRS{queue_depth}=="30"
    ATTRS{vpd_pg83}==""
    ATTRS{model}=="0002-1NA17Z     "
    ATTRS{evt_media_change}=="0"
# truncated due to length
[...]
```

With this I could find at least the **ATTRS{vendor}=="ST8000AS"** which is kind of unique for my setup.
If you search for this string on the internet you get the model number for my HDD.
Since I only have this one it is okay for my usecase right now.  
When I happen to buy another one of these I have to be carefull not to automaticly use the wrong HDD.

So with that I did write the follwing udev rule:

{{< gist ajfriesen 1e4519efa809db4b452543bcf89950fe "10-local.rules" >}}

This will start the configured systemd service when the drive with that specific attributes is added added to the system.

{{% alert note %}}
When testing udev rule change one thing at a time.
At best, you just test with something really safe and easy:
```
KERNEL=="sd*", ACTION=="add", SUBSYSTEM=="block", ENV{SYSTEMD_WANTS}="backup-decrypt-mount.service"
So that you know something will happen.
```
{{% /alert %}}

{{% alert warning %}}
Also be carefull with systemd version!

My version:
```bash
systemctl --version
systemd 245 (245.5-2-arch)
```
Some distros do have a way older systemd version and systemd has a lot of breaking changes.
Like this ENV statement I am using for calling a systemd service.
To check which option you can use you have to check man systemd.
{{% /alert %}}

So let´s move to systemd then.

## Create a systemd service

{{< gist ajfriesen 1e4519efa809db4b452543bcf89950fe "backup-to-hdd.service" >}}

Do not forget to enable the systemd service with:
```
systemctl enable backup-to-hdd.service
```

## backup script

This script is really just a small rsync command for now.

{{< gist ajfriesen 1e4519efa809db4b452543bcf89950fe "backup-to-hdd.sh" >}}

The source is my raid where I have a backup directory.
Everything which is in this directory shall be copied over to the HDD in the toaster.
How that data goes there may be covered in a later post.

The destination is the mountpoint and folder for my HDD in that toaster.
The idea behind that is having a cold storage which is not always connected to the server and also may be stored in another building.
(Good luck with that when working from home :wink:)

[^1]: [Arch Wiki - udev](https://wiki.archlinux.org/index.php/Udev)
[^2]: [Arch: Automount encrypted sdcard – udev + systemd](https://technik.blogbasis.net/arch-automount-encrypted-sdcard-udev-systemd-09-10-2015)
[^3]: [Writing udev rules by Daniel Drake](http://www.reactivated.net/writing_udev_rules.html)
[^4]: [How do I use ENV{SYSTEMD_USER_WANTS}= in udev rule?](https://superuser.com/questions/1033270/how-do-i-use-envsystemd-user-wants-in-udev-rule)