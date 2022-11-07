# LPIC 2

##

### 200.1 Resources and Measures for Problem Resolution

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
```