# LPIC1 Course Notes

## DAY 01

VirtualBox

Alma Linux/Debian
Rocky/Debian

No GUI - 98% of certification focused on CLI
How to install GUI via CLI 

Recommended distro: for LPIC1 any distro is OK

Fedora
Ubuntu - is less recommended for LPIC1 certification, focused on ease of use - introducing distro specific tools (snap, plan)
RedHat
Debian
CentOS
Linux Mint
Rocky Linux
Alma Linux
Open SuSe

Ubuntu ll alias - ll is aliased to `ls -alF'

```Bash
# To view alias target
type <ALIAS>
type ll
# ll is aliased to `ls -alF

# -a, --all - do not ignore entries starting with .
ls -a

# sh prompt - $
# bash prompt - username@HOSTNAME:~$

```

### Exam Content

101: System Architecture
102: Linux Installation and Package Management
103: GNU and Unix Commands
104: Devices, Linux Filesystems, Filesystem Hierarchy Standard

### Essential commands

```Bash
# Current user
whoami
# Current host
hostname
# Path to working directory
pwd
# Help
## Usage manual
man
## Read info documents - information
info
## help command - when no man, whereis do not display location for such commands
help <command>
help fg
### those command are "shell bulletin"
type fg
### others are "hashed"
type mkdir
## --help switch for command

# Check whether it is file or directory and identify file type
file filename

# man section
# It is possible to create man files for your own commands/scripts or edit existing man files
# /usr/share/man - default main man directory, with sub-directories for man sections
# 1 : User command (executable programs or shell commands)
# 2 : System calls (functions provided by the kernel)
# 3 : Library calls (functions within program libraries)
# 4 : Special files (usually found in /dev)
# 5 : File formats and conventions eg /etc/passwd
# 6 : Games
# 7 : Miscellaneous (including macro packages and conventions),
# 8 : System administration commands (usually only for root)
# 9 : Kernel routines [Non standard]
```

In Linux everythnig is file.

Use of vi is an exam topic (103.8 Basic file reading).

- grep
- egrep
- fgrep
- sed
- regex

Certification is focused on bash interpreter.

### Bash KB shortcuts

### history

Stored in .bash_history hidden file (ls -A). Stored in memory when session closes correctly/gracefully (with exit) it gets written into .bash_history file - 1 file per user, from multiple sessions (reload on session start).

```Bash
# Clear up history - session buffer clear up
# for full clearup clear both buffer and file
history -c
# Specific line removal
history -d N
# Removing history file
rm -r .bash_history
# cat concatenates
cat .bash_history
# fmt allows formatting
fmt .bash_history
```

```Bash
# which only searches for executables / where does this command comes from? - Returns full path of (shell) commands
which ls
# whereis searches for any files - locates binary, source, and man page files for a command
whereis ls
```

**sudo = Switch User and Do**

```Bash
# sudo elevation - switch user and do
sudo COMMAND
# switch to sudo user
sudo su
# System info
uname -a

# echo
# double quotes - vars expanded
echo "This is my $HOME"
# single quotes - vasrs not expanded, literal output
echo 'This is my $HOME'

# alias
alias ALIAS=COMMAND
# to make it persistent add alias to bash.rc
```

## DAY 02

rm .bash_history

### Absolute/relative paths

Absolute path - starts from root /, in Win from drive letter.

In Linux we have 1 root, in Windows every drive has its own separate root starting from drive level (no common root).

### Data flows

Data flows/channels.

stdin[0] - input stream
stdout[1] - output stream
stderr[2] - error stream

```Bash
echo "Hola mundo"
# Write to txt file, overwriting existing file
echo "Hola mundo" > out.txt
# Write to txt file, appending to existing file
echo "Hola mundo" >> out.txt
# Below will output error to stderr[2] and nothing to file/stdout[1]
echno "Hola mundo" > out.txt
# Sending to 2 streams
find / -name interfaces 1> match.txt 2> error.txt
```

```Bash
/usr/bin/df -hT | grep -v loop
```

**/dev/null** - "black hole" - discards everything you send into it

Pipe - pass stdout of one command to stdin of another one.

ifconfig - deprecated package, alternative ip.

tr command - translate/delete characters, can be useful to replace space with some other symbol (" " > ";") to see more easily double spaces.

```Bash
# Replace char/letter
echo "Hola" | tr "H" "Z"
```

cut command - cut part of the file based on specified delimiter and column N ()

grep - print out lines which match patterns
```Bash
# Look up for user
cat /etc/passwd | grep mikhail
# Count cosequtive unique lines
uniq -c
# Order to make data consequtive
sort
```

/var/log/secure

### Aliases

alias - shows configured aliases


~/.bashrc, inside of it:

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

~/.bash_aliases

### Check/ban IP

https://www.ripe.net/ - entity which manages public IPs in EU, Russia

ripe.dm

whois

fail2ban
https://www.arsys.es/blog/instalar-fail2ban

/var/log/auth.log

clear / ctrl + L - clears screen by scrolling up
reset - deletes

### 103.2 Process text streams using filters

less is interactive command, you will need to exit yourself

less
/word_to_search

more is noninteractive command it exists itself

more

head
tail

tail -f

tac - cat backwards :)


```Bash
bzcat
cat
cut
head
less
md5sum
nl
od
paste
sed
sha256sum
sha512sum
sort
split
tail
tr
# uniq counts only consequitive lines as occurences, non consequtives will be counted separately
# can be used in combination with sort to workaround non-consequitive occurrences problem
uniq
wc
xzcat
zcat

# Number file lines, enter/line return counts for new line for the system
nl
cat -n file.txt
# in vi set list command will show line return as $ / set nolist to disable this
# cat -e to show end of line as $
cat -e file.txt

# sort by column, -n to specify numeric sort
sort -k 2 -t":" -n data.txt
# -R for random sort
alias voluntarion='sort -R students | head -l'
```

### Packaging/compression

Packaging - tar - no compression, just package into one file
Compression - gzip, bzip2, xz

To group files into package you package them, then compress

tar czf package.tar file*

File globbing VS regular expressions

Parameters: - Unix style, -- GNU style

## DAY 03