# LPI LPIC-1 System Administrator

Includes/requires 2 exams - 101-500 & 102-500

## LPIC-1 Exam 101

- Created in 1999 ~ about 10 years after 1st Linux kernel release
- In 2010 LPIC teamed up with CompTIA to create combined set of exams (LXO-103 & LXO-104), passing both of those lead to LPIC-1 & Linux+ certifications, but those exams retired in 2019
- Entry level, distribution agnostic
- Current version - V5, not tied to CompTIA Linux+
- 60 questions - multiple choice & fill-in the blanc / 90 minutes
- 5 years validity

4 main topics:

- System Architecture
- Linux Installation & Package Management
- GNU & Unix Commands
- Devices, Linux Filesystem, Filesystem Hierachy Standard

[Exam 101 Objectives list](https://www.lpi.org/our-certifications/exam-101-objectives)

### 101.1 Determine and configure hardware settings

#### Pseudo File Systems (PFS)

File System - method of laying out files and folders on a physical HDD.
In Linux everything is a file/seen as a file - including devices.
Pseudo File System
- do not exist on a physical hard disk
- only exist in RAM while system is running
- on boot kernel creates pseudo file system
Two primary pseudo file system locations in Linux
- /proc - contains PID subfolders; this PFS contains information about the **processes** running on a system, processes are listed by PID, with HW and process data both in the same directory structure
- /sys - contains information about the system's **hardware and kernel modules**. No process information listed here

We can see local documentation on lon the /proc PFS using *man proc* command.

/proc/cpuinfo file contains info about CPU - created in real-time and lives in memory
/proc/1 - init process directory, initialization system

/proc PFS is useful source of info about your system and running processes.


**/sys** - HW and kernel info is exposed in plain text files within this PFS/provides info about the kernel (hardware devices and their device drivers) through user space
- block
- bus
- class
- dev
- devices
- firmware
- fs - file systems - cgroup, xfs etc., inside of FS sub directory we can find partitions sub directory etc. P
- hypervisor
- kernel
- module
- power

**Everything is seen by Linux as a file.**

#### Working with Kernel Modules

Linux Kernel
- Core framework of the OS
- Provides a way for the rest of the system to operate with HW, memory, networking, and itself
- Linux kernel is monolythic
    - Kernel handles all memory management and HW device interactions
    - Extra functionality can be loaded and unloaded dynamically through kernel modules
    - Ensures that the system won't need to be booted into different kernel image for added functionality
- Many 3rd-party Linyx kernel modules are device drives

uname - returns basic Kernel info
uname -m - bitness
uname -rm - kernel release and bitness/machine type
uname -a - all info

lsmod - list Kernel modules, along with modules which are dependent on listed module
modinfo <module name> - get info about specific Kernel module
modprobe -r <module name> - unload Kernel module (use with caution, can break the system)
modprobe <module name> - load Kernel module

#### Investigating Hardware

/dev, udev, D-Bus

udev Linus service acts as a device manager
- udev detects newly connected HW (e.g. disk) and passes info about it through dbus service to /dev PFS
D-Bus service - data bus service
- passes all information about everything that goes on within the Linux system to other services and devices
new device then gets attached to /dev PFS which contains actual handles to all devices that connected to the system
other apps trying to access HW, e.g. disk with lsblk (ls block, to list all the block devices) it goes through D-Bus to /dev PFS to get info about device

ls /dev/cpu - will show all cores
ls /dev/dri - video cards
/dev/nvme0 - SSD disk
/dev/nvme0n1 - SSD disk partition
/dev/nvme0n1p1 - SSD disk partition

/dev/sda - USB hard drive

lspci - list all PCI devices currently attached to the system (queries /dev and returns what is connected to PCI bus) - USB controllers, NICs, memory controller, sound/video cards, card riders etc.

lspci -k - to see which HW components use which Kernel modules (Kernel driver in use, Kernel modules)
lspci -k -v - to see even more info (verbose) - capabilities

lsusb - to list all USB devices attached to the system (disks, root hubs/controllers)
lsusb -v - verbose
lsusb -t - tree view to see which USB device attached to which controller

lscpu - list CPU info

lsblk - list hard disks (block devices) - NAME / MAJ-MIN / SIZE / RO / TYPE / MOUNTPOINT
lsblk -f - to get file system used by each partition (FSTYPE)

### 101.2 Boot the System

#### The Linux Boot Sequence

>>>

### 101.3 Change Runlevels/Boot Targets and Shutdown or Reboot the System
