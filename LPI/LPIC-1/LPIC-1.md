# LPI LPIC-1 System Administrator

LPIC-1 includes 2 exams includes/requires 2 exams - 101 & 102

## LPIC-1 Exam 101

- Created in 1999 ~ about 10 years after 1st Linux kernel release
- In 2010 LPIC teamed up with CompTIA to create combined set of exams (LXO-103 & LXO-104), passing both of those lead to LPIC-1 & Linux+ certifications, but those exams retired in 2019
- Entry level, distribution agnostic
- Current version - V5, not tied to CompTIA Linux+
- 60 questions - multiple choice & fill-in the blank / 90 minutes
- 5 years validity

4 main topics:

- System Architecture
- Linux Installation & Package Management
- GNU & Unix Commands
- Devices, Linux Filesystem, Filesystem Hierachy Standard

[Exam 101 Objectives list](https://www.lpi.org/our-certifications/exam-101-objectives)

Contents/topics covered:
- Linux system administration
- System Architecture
- Installation of a Linux sustem and installing software
- Commonly used Linux commands
- Understanding Linux file system and how it works with different types of hardware

Prerequisite knowledge:

- LPI Linux Essentials
- Some familiarity with Linux

## 101.1 Determine and configure hardware settings

### Pseudo File Systems (PFS)

File System 
- method of laying out files and folders on a physical disk

Pseudo File System
- do not exist on a physical hard disk
- only exist in a RAM while system is running
- on boot kernel creates pseudo file system

In Linux everything is a file/seen as a file - including processes, folders and devices.

Two primary pseudo file system locations in Linux are /proc and /sys

#### /proc

- /proc - contains **PID subfolders**; this PFS contains information about the **processes** running on a system, processes are listed by PID, with HW and process data both in the same directory structure
  - PID folders for processes
    - 1 - first process which starts on system's boot / init process directory, initialization system
      - 1/cmdline - command related to process 1 - systemd
  - cpuinfo file - contains info about CPU, get created in real-time and lives in memory
  - meminfo file
  - partitions file
  - uptime file
  - version file

- /proc PFS is useful source of info about your system and running processes.

#### /sys

- /sys - contains information about the system's **hardware and kernel modules**. No process information listed here.
  - /block
  - /bus
  - /class
  - /dev
  - /devices
  - /firmware
  - /fs - file systems used on machine, - cgroup, xfs etc., inside of FS sub directory we can find partitions sub directory etc.
- hypervisor
    - cgroup
    - pstore
    - resctrl
    - selinux
    - xfs
      - xvda1
        - error
        - log
        - stats - partition's file system statistics
  - /hypervisor
  - /kernel
  - /module
  - /power

**/sys** - HW and kernel info is exposed in plain text files within this PFS/provides info about the kernel (hardware devices and their device drivers) through user space

We can see local documentation on the /proc PFS using *man proc* command.

**Everything is seen by Linux as a file.**

### Working with Kernel Modules

#### Linux Kernel

Linux Kernel
- Core framework of the OS (GNU/Linux)
- Provides a way for the rest of the system to operate with its HW, memory, networking, and itself (Kernel's communication with its various subsystems)
- Linux kernel is **monolythic kernel**
    - Kernel handles all memory management and HW device interactions
    - Extra functionality can be loaded and unloaded dynamically through kernel modules
    - Ensures that the system won't need to be rebooted into different kernel image for added functionality
- Many 3rd-party Linyx kernel modules are device drives

#### uname

```Bash
## uname - returns basic kernel info
uname -m # bitness
uname -rm # kernel release and bitness/machine type
uname -a # all info
# Sample uname -a output
Linux WORKSTATION1 5.10.102.1-microsoft-standard-WSL2 #1 SMP Wed Mar 2 00:30:59 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

#### lsmod, modinfo, modprobe

```Bash
# lsmod - list modules
lsmod # list Kernel modules, along with modules which are dependent on listed module
# output includes module name, size and used by information - which lists other modules which use this module
modinfo <module name> # get info about specific Kernel module
# Load/unload kernel modules
modprobe -r <module name> # unload Kernel module (use with caution, can break the system)
modprobe <module name> # load Kernel module
```

### Investigating Hardware

#### Linux Device Management

**/dev, udev, D-Bus**

- **/dev** - PFS which contains information on all of the system's connected HW
- **udev service** - Linux service which acts as a device manager for the Linux kernel, links information on system HW to /dev
  - detects newly connected HW (e.g. disk) and passes info about it through dbus service to /dev PFS
- **D-Bus service** - data bus service, sends data messages between application, a conduit of information about what is going on in the system, udev utilizes dbus to notify users and the system when new HW is attached
  - passes all information about everything that goes on within the Linux system to other services and devices
  - new device gets attached to /dev PFS which contains actual handles to all devices that connected to the system
  - other apps trying to access HW, e.g. disk with lsblk (ls block, to list all the block devices) it goes through D-Bus to /dev PFS to get info about device

**/dev**

/dev/cpu - numbered subfolders for each CPU core, microcode
/dev/dri - video cards
/dev/nvme0 - SSD disk
/dev/nvme0n1 - SSD disk
/dev/nvme0n1p1 - SSD disk partition
/dev/sda - USB hard drive

#### Listing devices - lspci, lsusb, lscpu, lsblk

```Bash
# lspci - Displays information on PCI devices attached
lspci # lists all PCI devices currently attached to the system (queries /dev and returns what is connected to PCI bus) - USB controllers, NICs, memory controller, sound/video cards, card riders etc.
lspci -k # to see which HW components use which Kernel modules (Kernel driver in use, Kernel modules)
lspci -k -v # to see even more info (verbose) - capabilities
# lsusb - Displays information on USB devices attached
lsusb # to list all USB devices attached to the system (disks, root hubs/controllers)
lsusb -v # verbose
lsusb -t # tree view to see which USB device attached to which controller
# lscpu - Displays information on processors on a system
lscpu # list CPU info
# lsblk - Displays information on all block devices on a system
lsblk # list hard disks (block devices) - NAME / MAJ-MIN / SIZE / RO / TYPE / MOUNTPOINT
lsblk -f # to get file system used by each partition (FSTYPE)
```

#### 101.1 Determine and configure hardware settings Recap

**sysfs** - is a pseudo file system that provides information about the kernel (such as hardware devices and their device drivers) through user space. The sysfs filesystem is **commonly mounted at /sys**. Typically, it is mounted automatically by the system.

**/proc** - is a pseudo file system that contains the processes that are currently running on the system, each with their own directory named after their PID. 

**udev** - is a device manager service for Linux.

**modprobe** - this command loads a kernel module into memory.
**modprobe -r <module name>** - this command unloads Kernel module (use with caution, can break the system)

**lsusb** - this command allows to view the hot-pluggable devices currently connected to your system; USB devices are considered hot pluggable (or hot swappable), and this command lists those that are connected to your system. 

## 101.2 Boot the System

### The Linux Boot Sequence

>>>

### 101.3 Change Runlevels/Boot Targets and Shutdown or Reboot the System
