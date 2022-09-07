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
:s/pattern1/pattern2 - replace in current line