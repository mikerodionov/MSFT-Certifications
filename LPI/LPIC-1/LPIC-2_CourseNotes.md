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