# LPIC 2

Vagrant

```Bash
sudo sh -c 'echo "* 10.0.0.0/8 192.168.0.0/16" > /etc/vbox/networks.conf'
sudo sh -c 'echo "* 2001::/6" >> /etc/vbox/networks.conf'
vagrant plugin install virtualbox_WSL2
```

## 200 Capacity Planning

### 200.1 Measure and Troubleshoot Resource Usage (weight: 6)

```Bash
iostat # used to monitor load of I/O of fevice and CPU
# -c - show CPU stats
# -d - show devices stats
# interval - N of second for refresh
# count - N of times that everyonre is shown
# device - to indicate device to consult

# Output devices
# tps - transfers per second
# kB_read/s - reads per second in KB
# kB_wrtn/s - writes per second in KB
# kB_read - total reads in KB
# kB_wrtn - total writes in KB

# /etc/profile
# TMOUT=30

vmstat # informs about the stats of virtual memory by process, page, bloques I/O and CPU utilization
vmstat 1 5

netstat # 
netstat -i # shows states of NICs

# CentOS 7 Systat
# Ubuntu - sudo apt install sysstat
iostat
mpstat
pidstat
tapestat
cifsiostat

# lsb_release
lsb_release -a

# top
```

```Bash
## ps - allows to see state of the processes, based on PFS /proc, i.e. directly reads files in this PFS
ps -efl | more
ps aux | more
ps -eo user,pid,tty,stat,command | more
ps -u root,apache
ps -u apache


pstree # shows the processes in the form of tree
pstree -AGu # -u, --uid-changes - show uid transitions
systemd-cgls # recursively show control group contents

# Process and jobs management - jobs, bg, fg, kill, killall, ps
# process with PID 1 = /sbin/init
# SIGTERM (signal 15)
# SIGKILL (signal 9)
# Jobs management - bg, fg, jobs
sleep 600
# Ctrl + Z = Stop
jobs
bg 1
# Ctrl + C = Terminate

ps aux | less # aux = all processes, all terminals, including with and withot TTY
# -a - all with tty, except session leaders
# -u, U, --user <UID> - effective user id or name
# x - processes without controlling ttys
ps aux | grep sleep
```

### 200.2 Predict Future Resource Needs (weight: 2)

Key Knowledge Areas

- Use monitoring and measurement tools to monitor IT infrastructure usage
- Predict capacity break point of a configuration
- Observe growth rate of capacity usage
- Graph the trend of capacity usage
- Awareness of **monitoring solutions** such as **Icinga2**, **Nagios**, **collectd**, **MRTG** and **Cacti**

The following is a partial list of the used files, terms and utilities:

- diagnose
- predict growth
- resource exhaustion

## Capacity Planning

```Bash
pidof apache2
ps ax | grep apache2 |grep -v grep
strace -p 4991
# Tomcat - servlets server
# JBoss, Weblogic - Java application servers

# Resources Management
procfs
sysfs

systat

# https://mmonit.com/monit/
```

## 201 Linux Kernel 

```Bash
cd /boot/
ls /boot/vmlinuz*
uname # Print certain system information, -a, --all = ALL
uname -r # -r, --kernel-release = print the kernel release
cat /proc/cmdline # to find the kernel path, but note that this path is relative to the root image at boot time, so / likely means /boot/ in the running system

# Kernel modules
ls /lib/modules
ls /lib/modules/3.10.0-327.el7.x86_64/kernel/fs/fat/

# Getting info about module and its dependencies
modinfo fat

insmod <module_path/module_name> # load module into memory

lsmod # shows modules loaded into memory

modinfo vfat # module info including path and modules it depends on (depends)
modinfo fat

insmod /lib/modules/2.6.18-8.el5/kernel/fs/fat/fat.ko
insmod  /lib/modules/2.6.18-8.el5/kernel/fs/vfat/vfat.ko
# lsmod |grep -i vfat


rmmod /lib/modules/2.6.18-8.el5/kernel/fs/vfat/vfat.ko
rmmod /lib/modules/2.6.18-8.el5/kernel/fs/fat/fat.ko
# lsmod |grep -i vfat

# modprobe vfat
# modprobe -r  vfat

## tar
# x --> extract
# c --> package
# v --> verbose
# f --> file
# -C --> directory to extract / -C, --directory=DIR = change to directory DIR

# Compile nginx from source code
yum install pcre-devel openssl openssl-devel make gcc wget -y

# Centos8, Centos7
yum groupinstall "Development Tools" -y      
yum install pcre-devel openssl openssl-devel -y

# Debian/Ubuntu : apt  install libssl-dev  build-essential

./configure --help --prefix=/etc
make
make install 

# Compiled app installation normally placed in the following directories:
/usr/local/bin
/usr/local/sbin

cd /root
wget http://nginx.org/download/nginx-1.19.6.tar.gz
tar xzfv nginx-1.19.6.tar.gz -C /opt



cd /opt/nginx-1.19.6

Mirar el fichero README 

# For Debian
./configure --without-http_rewrite_module --without-http_gzip_module --with-http_ssl_module
make
make install 



# For CentOS
./configure --help
./configure --with-http_ssl_module  
make
make install 

# Verify installation

/usr/local/nginx/sbin/nginx -V

# Start nginx
/usr/local/nginx/sbin/nginx    
netstat -putan | grep -w 80

curl http://<IP>

/usr/local/bin
/usr/local/sbin

# Stop nginx
killall nginx
```

### sysctl

```Bash
ls /proc/sys
# dev - config params for connected devices
# fs - params related with files, inodes, quota, etc.
# kernel
# net - network config params
# vm - virtual memory configuration

sysctl
# -a - show all variables
# -w / --write - enable writing a value to variable, changes are load on restart, permanent changes has to be writtent into /etc/sysctl.conf
# -p / --load[=<file>] - read values from a file to verify that settings are correct

journalctl # Query the journal
```

```Bash
# List all systemd units of the type service
systemctl list-units --type service --all

# Configure auto start of Apache in systemv and systemd
# systemv
chkconfig --level 3 httpd on
ln -s /etc/init.d/httpd   /etc/rc3.d/S85httpd
# systemd
systemctl # query or send control commands to the systemd manager
systemctl enable httpd

# Start httpd service with systemv and systemd
service httpd start
/etc/init.d/httpd start
# systemd
systemctl start httpd

# Checl of Apache is configured for autostart with systemv and systemd
chkconfig --list httpd apache2ch # systemv
systemctl  is-enabled httpd apache2 # systemd

#
Cambiar al target o runlevel 3 tanto en systemd como en systemv en caliente
init 3
telinit 3
systemctl isolate multi-user.target

Averiguar en que target arranca el sistema en systemd
cat /etc/inittab
systemctl get-default

Ver con journalctl los logs de error del servicio sshd
journalctl -p err -u sshd

Ver todos los logs de error del sistema con journalctl
journalctl -p err

Ver los logs del service httpd y mariadb con journalctl
journalctl -u mariadb -u httpd

Ver los logs de los ultimos 5 minutos del servicio sshd
journalctl --since '5 min ago' -u sshd

Comando iostat para ver estadisticas de cpu
iostat -c 1 5

Comando iostat para ver estadisticas de dispositivos
iostat -d 1 5

Parametro del comando vmstat para visualizar marca de tiempo
vmstat -t

Parametro del comando netstat que permite obtener el estado de las tarjetas de red
netstat -i
```

## System Startup - SystemD

```Bash
# /usr/lib/systemd/system/: units distributed/installed via RPM packages
# /run/systemd/system/: units created at execution time, have precedence over previous directory
# /etc/systemd/system/: units created and administered by sys admin, this directory has precedence over previous directory 

yum install git -y
cd / 
git clone https://github.com/agarciafer/lpic2.git
cd /lpic2
iniciar-supervisamem supervisamem

cd /etc/systemd/system
vi supervisamem.service


## Webmin

# https://www.webmin.com/
# Webmin is a powerful and flexible web-based server management control panel for Unix-like systems. Webmin allows the user to configure operating system internals, such as users, disk quotas, services or configuration files, as well as modify and control open-source apps, such as the Apache HTTP Server, PHP or MySQL.
```

## Linux Disks

```Bash
## Disk types

# IDE disks with IDE controllers (aka PATA, Parallel Ata or ATAPI) 
# they are named hdX: • hda: IDE0, Master • hdb: IDE0, Slave • hdc: IDE1, Master • hdd: IDE1, Slave

# SCSI, SATA, USB, FIREWIRE, etc.
# • sda: first SCSI disk • sdb: second SCSI disk • sdc: third SCSI disc • etc. 

# SCSI standard distinguish between different devices, hence CD-ROM, DVD, HD-DVD, BlueRay readers and recorders do not have the same name
# Readers and recorders named srX (sr0, sr1, etc.).

## fstab

# df - Show information about the file system on which each FILE resides, or all file systems by default.
# -h, --human-readable  print sizes in human readable format (e.g., 1K 234M 2G)
# -T, --print-type      print file system type
df -hT
```

### Partitioning - Creating and Mounting a Partition

```Bash
fdisk -l #  query information about the new data disk
fdisk /dev/sdb # enter fdisk to partition the new data disk
n # create new partion, then accept defaults to create primary partition using all disk space
    p # primary
    1 # partition number
    # first secrtor, default = 2048
    # last sector
p # view details about partition
w # write changes to the partition tab
```

### XFS

```Bash
# Repair XFS FS, no bad blocks verification
xfs_repair /dev/sdf1
# Bad blocks verification
badblocks /dev/sdb1
xfsdump # backup xfs parition, The –L [session label] option allows you to assign a label to the backup
xfsdump -f /cs /dev/sdb1
file /cs
xfsrestore # restore xfs partition
xfsrestore -f /cs /logs
xfsrestore -f /cs /logs-backup/ # restore to different folder/mount point
# restore file by file - ls, add, extract
xfsrestore -f /cs -i -v silent /logs
# df - Show information about the file system on which each FILE resides, or all file systems by default.
#  -h, --human-readable  print sizes in human readable format (e.g., 1K 234M 2G)
#  -T, --print-type      print file system type
df -hT
```

### swap

```Bash
free -h # memory and swap used/total size
swapon -s # swap location
cat /proc/swaps # PFS to see swap location
lsblk # to see swap partition
# type 82 = swap
mkswap /dev/sdb2 # volume or file
swapoff /dev/sdb2 # volume or file
fstab # persistent configuration

# controlling swap location priorities
# /etc/fstab - static file system information - pri=NN
#/dev/mapper/ubuntu--vg-swap_1 none            swap    sw,pri=90              0       0 

#
cat /proc/sys/vm/sappiness
vm.swappines = 10
```

### LVM

