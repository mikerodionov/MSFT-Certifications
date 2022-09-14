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

```Bash
ls /tmp/
mv /dir1/file1 /dir2/file2
# -f to suppress confirmation prompt
mv -f /dir1/file1 /dir2/file2
# "mv -i" = interactive = ask for confirmation, on RedHat distros mv is an alias to "mv -i"
# to create multple sub dirs _101, _102, etc.
#  -p, --parents - no error if existing, make parent directories as needed
mkdir -p lpi/exam_{101,102,201,202}/topics
# Remove the DIRECTORY(ies), if they are empty.
rmdir
# Remove directory with files
rm -rf /dir
# ls
#  -A, --almost-all - do not list implied . and ..
# -h, --human-readable - with -l and -s, print sizes like 1K 234M 2G etc.

# File Globbing, 3 types of globs (aka wildcards)
# * - any combination of any number of characters
# ? - any single character
# [] - matches the combination by exactly one character [A2] will return anything that contains A or 2
# ! - exclamation mark excludes characters from the list within the square bracket [!BC]
# ranges [a-z] and [0-9]

# grep with regex
# ^ beginning of a line
# $ end of a line
# all commented lines
grep "^#"
# all except comments - use -v = invert match
# remove comments (grep -v "^#") and empty lines (grep -v "^$")
cat .bashrc | grep -v "^#" | grep -v "^$"
grep -v "^#" .bashrc | grep -v "^$"

# File globbing VS regular expressions

# Search any number of spaces with end of line (empty line with N spaces) * = 0 to N occurrences
grep "^ *$" test.txt
# Eleminate such lines
grep -v "^ *$" test.txt

grep [3-5] /etc/passwd

# . any character in specific position only "1.3" will match 123, 113, 153 etc.
# .* - asterisk refers back to preceding char = any char any number of times from 0 to N

# Extended regular expressions
# grep -E =  -E, --extended-regexp PATTERNS are extended regular expressions
# +, ? - 1 or N times occurrence of preceding char or pattern, {} - number of repetition of preceeding char or pattern
# {1,3} = {min,max}

# () - groups of patterns
# Search for IPv4
# Look for 1 or 3 digits followed by literal dot and repeat 3 times
grep -E "([0-9]{1,3}\.){3}" test.txt
# 
grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" test.txt

# | - use more than one pattern (one or another)
grep -vE "^$|^#" test.txt

# non system users will have UID above 1000
# Users with home dir
 grep home.* /etc/passwd
```

#### 103.8 Basic file editing

```Bash
# Opening multiple "windows" inside of the same terminal
tmux
screen
# Ctrl + B, c - create, n - navigate, d - detach
tmux new -s "TMP"
tmux list-sessions
tmux a "TMP"
```

##### vi

3 modes:
- NAVIGATE
- INSERT
- EXECUTE

###### NAVIGATION

```
Esc Esc Esc : q !

Ctrl + C - will leave file.swp - with unsaved changes

H J K L = arrows, up and down go by line (EOL based, not visual)

gg - go to the first line
G - go to the last line
[N]G - go to line N
:N go to line N
/pattern + Enter - search for word descending, n to go the next occurence, N to go to
/?pattern - search for word ascending, n to go the next occurence, N to go to

0 - go to the beginning of a line
$ - go to the end of the line
w - jump by word/next word

u - undo
. - repeat

Go to Nth word:
[N]w next word
[N]b previos word

US KB ñ = :

dd - delete line (cut)
dw - delete word (cut)

Ndd - delete N lines (cut)
Ndw - delete N words (cut)
```

p - paste after
p - paste before

y - yank
[N]yy - copy N lines
[N]yw - copy N words

###### INSERT

To navigate - exit INSERT mode

To enter into insert mode:
i - insert text in cursor position
a - insert text after cursor
o - insert new line after current

I - insert text in the beginning of current line
A - insert text in the end of the line
O - insert new line before current

###### EXECUTION

:w - Write/Save
:q - Quit/Exit
:w [PATH] - Save as
ZZ  - Save & quit
:! command - open shell and run command
:r! command - open shell, run command and return output into document
:s/pattern1/pattern2 - replace first occurrence in current line
:s/pattern1/pattern2/g - replace all occurences in current line
:1,$s/pattern1/pattern2/g - replace all occurences in all lines

## DAY04

screen & tmux

grep

```Bash
echo "Este o este estereo estereotipo" | grep -E '((Est*)|(est*))'
# sin -E
echo "Este o este estereo estereotipo" | grep 'Este\|este'
# -i = case insensitive
echo "Este o este estereo estereotipo" | grep -i 'este'
# -w = search for word
echo "Este o este estereo estereotipo" | grep -iw 'este'
# -o = print  only  the  matched  (non-empty) parts of a matching line, with each such part on a separate output line
echo "Este o este estereo estereotipo" | grep -io 'este'
# count pattern match
echo "Este o este estereo estereotipo" | grep -iwo 'este' | wc -l
# -c = count lines with matching pattern (only lines, no matter how much it repeats per line)
echo "Este o este estereo estereotipo" | grep -iwc 'este'
# -q = quiet
# state variable, exit 0 (match found), exit 1 (match not found), exit 2 - error
grep -q 'este' info.txt

# -B n = show N lines preceding to match
grep -B 1 -iw 'banc[ao]' 

# recursive search through files for match
grep -r 'nameserver' /etc/
# also show line number
grep -nr 'nameserver' /etc/

# egrep, fgrep, rgrep = obsolete commands
# egrep = grep -E, fgrep = grep -F, rgrep = grep -r

# vi
# show line number - :set number
# hide line number - :set nonumber

# sed - replacements
# by default sed replace only 1st match on the line

sed s/[Ee]ste/norte info.txt

# Replace e with E on every line which has "b"
sed '/b/' 's/e/E/' colors.txt
```

```template.conf
<VirtualHost *:80>
    ServerName template.com
    ServerAlias webmaster@template.com

    DocumentRoot /var/www/html/template

    AccessLog /var/log/apache2/template-access.log
    ErrorLog /var/log/apache2/template-error.log
</VirtualHost>
```

```Bash
# Links - 2 types: soft & hard
# soft/weak link - direct access (via small independent shortcut file which takes small amount of disk space)
# hard link - do not use disk space
df -hT
# create 2 GB file
dd if=/dev/zero of=file1 bs=1M count=2000
ln file1 file2
ln -s file1 file_soft
ls -lh
rm file1
ls -lh
# inode - Linux FS works with inodes - structures which contain files' metadata which include pointers list
# Linux must allocate an index node (inode) for every file and directory in the filesystem. Inodes do not store actual data. Instead, they store the metadata where you can find the storage blocks of each file’s data.
# Check the inode number in a specific file
ls -i
# Check the inode usage on FSs
df -i

# Search by name
find / -name "TestFile"
# Search by type
# f = file
find / -type f
# i = inum
find / -type f -inum 130775 2> /dev/null

# Processes in Linux
# LPIC1 focuse exclusively on top
top
# top load average should be below 1
# top KB shortcuts
# k - kill process

# swap
# free -h to seee available memory & swap info
# allows to see if swap is being used while RAM is unused
free -h
# buffers data waiting to be placed to disk - CPU maybe working on them still
# cache data stored to disk - already processed data
free -h -w

# bashtop
bashtop

# ps - report a snapshot of the current processes, within current session
# ps -ax - report a snapshot of the current processes, within all sessions

# who - Print information about users who are currently logged in.

# zombie process - process which is not terminated and do not have parent process, state code Z
# S - sleep
ps -ax
ps -ax -o pid,stat | grep Z

ps -ax | grep ping

ps -ax -o pid,ppid,tty,cmd | grep PID

# zombie clear up
kill -HUP $(ps -A -ostat,ppid | grep -e '[zZ]'| awk '{ print $2 }')

# kill -2 = send signal 2 ~ Ctrl +C, do not work for processes running in foreground
# kill -19 = pause process
# kill -18 = resume process

# jobs -  Lists the active jobs.  JOBSPEC restricts output to that job.
# Without options, the status of all active jobs is displayed.
jobs --help

# fg PID - bring process to foreground

# Kill key statuses
# 2 - SIGINT - the signal sent when we press Ctrl+C. The default action is to terminate the process. However, some programs override this action and handle it differently; "interruption request sent by the user"
# 9 - SIGKILL - "kill"; When a process receives SIGKILL it is terminated. This is a special signal as it can’t be ignored and we can’t change its behavior.
# We use this signal to forcefully terminate the process. We should be careful as the process won’t be able to execute any clean-up routine.
# 15 - SIGTERM - "exit"; terminate the process but specifically requesting to finish it, SIGTERM is the default signal when we use the kill command. When we send SIGTERM, the process sometimes executes a clean-up routine before exiting and it also can be handled to ask for confirmation
# 18 - SIGCONT - "continue" 
# 19 - SIGSTOP - "pause"
```

```Bash
# Loop example
for i in apple manazana orange ; do echo $i ; done
```

## DAY05

File permissions

chmod

9 bits, 3 groups [---][---][---]

- owner rwx
- group owner rwx
- others rwx

```Bash
# To see user ID and user's groups IDs
id username
# Remove execute rights
sudo chmod -x /usr/bin/firefox
# View permissions
# Current dir
ls -ld
# Specific file within current dir
ls -ld file1.txt
# X permission required to traverse directory content ~ access to a directory (user will be able to view directory contentent but not to enter into it
# In Linux directory is a file which contains 2 columns - resource/file name and corresponding i-node number
# Root directory is always i-node 2, all others have random number
# To view file in directory we look into folder i-node and then move on to file i-node
# Directory R = check directory content
# Directory X = access directory conten
```

Permission management - symbolic notation or base-8 notation (0-7)

| base-8 | binary | RWX |
|--------|--------|-----|
| 0      | 000    | --- |
| 1      | 001    | --x |
| 2      | 010    | -w- |
| 3      | 011    | -wx |
| 4      | 100    | r-- |
| 5      | 101    | r-x |
| 6      | 110    | rw- |
| 7      | 111    | rwx |

```Bash
# Change file owner
chown user file
```

Symbolic notation

Useful when you need to add/remove permissions for entity

echo 'echo "Hello world!"' > script.sh
chmod +x script.sh
./script.sh

Entities
u - User
g - Group
o - Owner

Operations
+ - add
- - remove
= - set

Permissions
r - Red
w - Write
x - Execute

./ - reference to current directory

```Bash
# Look for files with 777 permissions
find /etc/ -type -f perm 777
# touch - updates 3 time stamps of existing file (access, modify, change) but not birt time stamp
touch file1.txt
stat file1.txt
```

Change group ownner

```Bash
# Change the group of each FILE to GROUP.
chgrp
# With chown
chown :group file
```

Practice:

```Bash
# PART1
# Obtener la MAC del interface ens3 usando el comando ip address show
ip a show eth0 | grep -Eoh '[a-fA-F0-9]{2}(:[a-fA-F0-9]{2}){5}' | grep -v 'ff:ff:ff:ff:ff:ff'
ip a show eth0 | grep ether | cut -d" " -f6
ip a show eth0 | awk '/[a-fA-F0-9]{2}(:[a-fA-F0-9]{2}){5}/'
ip a show | grep ether | awk '{print $2;}'
# Mostrar la hora actual, solamente la hora
# Only current hour
date "+%H"
# Current time HH:MM:SS
date "+%T"
# cut issue - column number will vary depending on region/format settings of your system
date | cut -d' ' -f 5
# Cambiar las minúsculas a mayúsculas de la palabra Hola
echo "HolA" | tr a-z A-Z
echo "HolA" | tr [:lower:] [:upper:]
# Mostrar el contenido de /etc/passwd sustituyendo los dos puntos “:” por espacios “ ”
cat /etc/passwd | tr ":" "  "
sed 's/:/ /g' /etc/passwd
# Líneas que contiene la cadena de caracteres admin en el fichero /etc/passwd
cat /etc/passwd | grep admin
# Muestra los campos 1, 3 y 7 del fichero /etc/passwd
cat /etc/passwd | cut -d":" -f1,3,7
# Muestra a partir del tercer campo del fichero /etc/passwd
cat /etc/passwd | cut -d":" -f3-
# Muestra el nombre de usuario, el UID y el shell del usuario pue
cat /etc/passwd | grep mikhail | cut -d":" -f1,3,7
# Número de líneas y de palabras del fichero /etc/passwd
cat /etc/passwd | wc -l
cat /etc/passwd | wc -w
wc -lw /etc/passwd
#Número de usuarios del sistema que utilizan /bin/bash como shell
grep /bin/bash /etc/passwd | wc -l
# More exact so that we look for the match before the end of the line
grep /bin/bash$ /etc/passwd | wc -l
# Muestra el fichero /etc/passwd en orden alfabético 
sort /etc/passwd
# Muestra el fichero /etc/passwd ordenado por UID en de forma inversa
sort -k3 -t":" -n -r /etc/passwd
# Pasar las vocales a mayúsculas de la palabra murcielago
echo "murcielago" | tr [aeiou] [AEIOU]
# Mostrar el contenido de la carpeta actual en formato largo, ordenado de forma ascendente por tamaño

# Mostrar por pantalla las líneas que contienen comentarios en el archivo /boot/grub/grub.cfg
# Enviar a un fichero las líneas del archivo /boot/grub/grub.cfg que no son comentarios
# Mostrar las líneas de un fichero que contienen la palabra BADAJOZ o HUELVA, numerando las líneas de salida
# Mostrar los ficheros que contienen la palabra "interface" en el directorio /etc/ y sus subdirectorios
# Buscar los 5 municipios con mayor superficie
# Buscar cuantos municipios hay en la provincia de Tarragona

# PART2
Muestra un salto de línea
Muestra una tabulación
Muestra las líneas del fichero /etc/network/interfaces numerándolas
Muestra las líneas del fichero /etc/network/interfaces numerándo sólo las líneas con contenido
Muestra las primeras 7 líneas del fichero /etc/passwd
Muestra las últimas 7 líneas del fichero /etc/passwd
Muestra las primeras 3 líneas de todos los ficheros .log del directorio /var/log
Busca dentro de los ficheros del directorio /var/log el patrón root
Ordenar, en orden inverso, las líneas del fichero /etc/passwd
Ordena el fichero /etc/passwd utilizando el campo UID como criterio de ordenación
Calcula el número total de titulos que hay en todos los ficheros del directorio /usr/share/data/peliculas/, descartando títulos repetidos
Indica el número de veces que aparece la palabra "molino" en el fichero "el_quijote.txt"
Dividir el fichero ripe.db en ficheros de 10M
Buscar la ubicación del comando fdisk dentro de todo el sistema
Identificar la versión del kernel que estamos usando
```

echo $$ - returns PID of bash

## DAY06