---
title: Bash Cheat Sheet
updated: 2025-04-06 22:02:35Z
created: 2025-04-06 21:37:03Z
---

# Bash Command Cheatsheet with Examples

## Getting Help

```bash
help echo                 # Shows help documentation for the echo builtin
man ls                    # Shows the manual page for the ls command
```

## History

```bash
history                   # Shows list of your recent commands like: 1 ls -la, 2 cd ~/Documents
^R (then type "git")      # Reverse search for commands containing "git" in history
!!                        # Repeats your last command (e.g., if last command was "ls -la")
!cd                       # Runs the last command that started with "cd"
```

## Variables

```bash
name="John Doe"           # Assigns the string "John Doe" to variable name
USER=temp ls -la          # Runs ls -la with USER temporarily set to "temp"
```

```bash
echo "$name"                      # Prints: John Doe
echo ${#name}                     # Prints: 8 (length of "John Doe")
echo ${name:0:4}                  # Prints: John (first 4 characters)
echo ${undefined:-"Not set"}      # Prints: Not set (since undefined isn't set)
echo ${count:="0"}                # Sets count to "0" if unset, then prints it
echo ${name+"is set"}             # Prints: is set (because name is set)
file="document.txt"
echo "${file#doc}"                # Prints: ument.txt (removes "doc" prefix)
echo "${file%txt}"                # Prints: document. (removes "txt" suffix)
echo "${name/John/Jane}"          # Prints: Jane Doe (replaces "John" with "Jane")
```

```bash
declare -i num=42         # Declares an integer variable
env                       # Prints all environment variables: USER=john, HOME=/home/john
export PATH=$PATH:/usr/local/bin  # Adds /usr/local/bin to PATH for this session and subprocesses
function greet() { local msg="Hello"; echo "$msg $1"; }  # msg only exists inside function
unset name                # Removes the variable name
```

## Quoting

```bash
echo 'The value is $HOME'   # Prints literally: The value is $HOME
echo "The value is $HOME"   # Prints: The value is /home/username
grep 'search term' file.txt # Passes literal 'search term' to grep
echo "Current user: $USER"  # Expands $USER: Current user: john
```

## Special Characters

```bash
echo ~                     # Prints your home directory: /home/username
echo $USER                 # Prints your username
find . -name "file*" -o -name "*.txt"  # Uses special chars for pattern matching
ls | grep txt              # Pipe (|) sends ls output to grep
echo {1..5}                # Expansion: prints 1 2 3 4 5
echo $(date)               # Command substitution: prints current date/time
```

## Options

```bash
set -e                     # Sets bash to exit immediately if any command fails
set +x                     # Enables command tracing (debugging)
shopt -s globstar          # Enables ** for recursive globbing
shopt -s nullglob          # Makes patterns that match no files expand to nothing
```

## Files and Directories

```bash
ls -hl --color=auto        # Lists files with human-readable sizes and colors
cd ~/Documents && pwd      # Changes to Documents dir, then prints current dir
pushd ~/Downloads && popd  # Temporarily changes to Downloads then goes back
cp -r src/ backup/         # Recursively copies src directory to backup
ln -s target link_name     # Creates a symbolic link
mv old.txt new.txt         # Renames old.txt to new.txt
rm -rf temp/               # Recursively removes directory temp
mkdir -p path/to/dir       # Creates directory and parent directories if needed
rmdir empty_dir            # Removes empty directory
rename 's/\.txt$/.md/' *.txt  # Renames all .txt files to .md
chown user:group file.txt  # Changes file owner and group
chmod 755 script.sh        # Makes script.sh executable (rwxr-xr-x)
umask 022                  # Sets default permissions for new files
touch newfile.txt          # Creates an empty file or updates timestamp
du -sh directory/          # Shows directory size in human-readable format
df -h                      # Shows disk space usage
file strange.bin           # Shows file type: "strange.bin: gzip compressed data"
stat file.txt              # Shows detailed file information
readlink symlink           # Shows where a symlink points to
which python               # Shows path to python in $PATH: /usr/bin/python
basename /path/to/file.txt # Prints: file.txt
dirname /path/to/file.txt  # Prints: /path/to
```

| Operator | Description |
| --- | --- |
| `-e` | File exists |
| `-f` | File exists and is a regular file |
| `-d` | File is a directory |
| `-L` | File is a symbolic link |
| `-r` | File has read permission |
| `-w` | File has write permission |
| `-x` | File has execute permission |
| `-s` | File is not empty (has size > 0) |
| `-O` | File is owned by the current user |
| `-G` | File’s group matches user’s group ID |
| `-N` | File modified since last read |

&nbsp;

## Redirecting I/O

```bash
echo "hello" > output.txt        # Writes "hello" to output.txt (overwrites)
echo "another line" >> output.txt # Appends "another line" to output.txt
grep error 2> errors.log         # Redirects error messages to errors.log
sort < unsorted.txt              # Reads input from unsorted.txt
find / -name "*.txt" &> all.log  # Redirects both stdout and stderr to all.log
find / -name "*.txt" 2>&1 | grep "interesting"  # Redirects stderr to stdout, then pipes
cat <<< "Hello world"            # Provides "Hello world" as stdin to cat
wget -q https://example.com >/dev/null  # Silences wget output
```

## Processes & Signals

```bash
sleep 60 &                 # Runs sleep command in background
bg %1                      # Continues job #1 in background after Ctrl+Z
fg %1                      # Brings job #1 to foreground
jobs                       # Lists all background jobs
# Press Ctrl+Z to suspend a process
sleep 10 && echo "Done"    # Sleeps 10 seconds then prints "Done" 
wait $PID                  # Waits until process with PID completes
kill -9 1234               # Forcefully terminates process with PID 1234
ps aux | grep firefox      # Lists all firefox processes
nohup python script.py &   # Runs script.py immune to hangups
trap 'echo "Exiting"' EXIT # Runs echo command when script exits
pgrep firefox              # Finds PIDs of firefox processes
pkill -9 firefox           # Kills all firefox processes
```

## Exit Codes ($?)

```bash
echo "Hello" && echo $?    # Prints: Hello then 0 (success)
ls /nonexistent && echo $? # Shows error then prints: 1 or 2 (failure)
command && echo "Success" || echo "Failed"  # Prints Success or Failed based on exit code
```

## Arithmetic

```bash
(( x = 5 + 3, y = x * 2 ))  # Sets x to 8, y to 16
echo $(( 10 / 3 ))          # Prints: 3 (integer division)
echo $(( 2**3 ))            # Prints: 8 (2 raised to power 3)
echo $(( x > 7 ? x : 7 ))   # Prints x if x > 7, otherwise prints 7
```

## Command Substitution

```bash
echo "Today is $(date +%A)"  # Prints: Today is Monday (or current day)
files=$(ls -1 | wc -l)       # Stores count of files in current directory
diff <(ls dir1) <(ls dir2)   # Compares output of two ls commands
```

## Shell Scripts

```bash
#!/bin/bash
# Simple example script
set -e  # Exit immediately on error
echo "Script name: $0"
echo "First argument: $1"
# ...more commands...
```

## Networking

```bash
hostname                        # Prints: computer-name
ifconfig | grep inet            # Shows IP addresses
ip addr show                    # Shows network interfaces and addresses
curl -O https://example.com/file.txt  # Downloads file.txt
wget -q https://example.com     # Quietly downloads the index page
nc -l 8080                      # Listens on port 8080
nc 192.168.1.100 80             # Connects to IP address on port 80
```

## Printing

```bash
lpr document.pdf               # Sends document.pdf to default printer
lpq                            # Shows current print queue
lprm 123                       # Removes print job 123 from queue
lpstat -p                      # Shows printer status
```

## Locales

```bash
echo $LC_ALL                   # Shows current locale for all categories
LC_ALL=en_US.UTF-8 date        # Runs date command with US English locale
```

## Filesystems

```bash
mount /dev/sdb1 /mnt           # Mounts device to /mnt directory
df -h /home                    # Shows disk space for /home partition
fsck /dev/sdb1                 # Checks filesystem integrity on device
```

## Key Bindings

```bash
# Press Ctrl+C to interrupt a command
# Press Ctrl+D to signal end of input
# Press Ctrl+Z to suspend a process
# Press Ctrl+L to clear screen
# Press Tab to autocomplete a command or filename
# Press Up/Down arrows to scroll through command history
```

## Globbing

```bash
ls ?ile.txt                   # Matches file.txt, pile.txt, etc.
ls [a-c]*.txt                 # Matches all .txt files starting with a, b, or c
echo {apple,banana,cherry}    # Prints: apple banana cherry
mkdir -p test/{a,b,c}         # Creates directories test/a, test/b, test/c
```

## System

```bash
free -h                       # Shows memory usage in human readable format
top                           # Shows real-time process information
ps aux | grep nginx           # Shows all nginx processes
sudo apt update               # Runs apt update as superuser
su - otheruser                # Switches to another user
```

## Arrays

```bash
fruits=( apple banana "kiwi fruit" )  # Declares array with three elements
echo "${fruits[1]}"                   # Prints: banana (second element)
echo "${fruits[@]}"                   # Prints: apple banana kiwi fruit
echo "${#fruits[@]}"                  # Prints: 3 (number of elements)
for fruit in "${fruits[@]}"; do echo "$fruit"; done  # Iterates through array
```

## Special Variables

```bash
command && echo "Exit status: $?"     # Prints exit status of previous command
function args_demo() { echo "Number of args: $#"; }; args_demo a b c  # Prints: 3
function print_all() { echo "All args: $*"; }; print_all a b c  # Prints: a b c
function print_sep() { for arg in "$@"; do echo "$arg"; done; }  # Prints args on separate lines
echo "Script name: $0"                # Prints the script name
./script.sh arg1 arg2                 # Inside script, $1 is "arg1", $2 is "arg2"
echo $PATH                            # Prints directories where commands are searched
echo $HOME                            # Prints your home directory
echo "Terminal size: $LINES x $COLUMNS"  # Prints terminal dimensions
echo $PS1                             # Prints your command prompt format
shift 2; echo $1                      # Shifts args left by 2, then prints the new $1
```

## Find & Xargs

```bash
find . -name "*.log" -type f -print0 | xargs -0 rm  # Safely removes all .log files
find /var/log -mtime +30 | xargs tar -cvzf old_logs.tar.gz  # Archives logs older than 30 days
find . -name "*.py" -print0 | xargs -0 grep "import"  # Finds imports in Python files
```

## Pipes

```bash
cat file.txt | grep "error" | wc -l   # Counts error lines in file.txt
ls -la | sort -k5 -n                  # Lists files sorted by size
echo "${PIPESTATUS[@]}"               # After piped cmd, shows status of each part: 0 0 0
mkfifo mypipe; cat file.txt > mypipe & grep "error" < mypipe  # Creates named pipe
```

## Scripting Examples

```bash
# Check if file exists
if [ -e /etc/hosts ]; then echo "Hosts file exists"; else echo "No hosts file"; fi

# Multiple conditions
if [ -d ~/Documents ]; then
  echo "Documents exists"
elif [ -d ~/docs ]; then
  echo "docs exists"
else
  echo "No document directory found"
fi

# Reading input line by line
cat file.txt | while read -r line; do echo "Line: [[${line}]]"; done

# Counting from 0 to 9
for ((i=0; i<10; i++)); do
  echo "Number: $i"
done

# Case statement
fruit="apple"
case $fruit in
  apple|Apple) echo "It's an apple" ;;
  banana) echo "It's a banana" ;;
  *) echo "Unknown fruit" ;;
esac

# Selection menu
select choice in "Option 1" "Option 2" "Quit"; do
  echo "You selected: $choice"
  [ "$choice" = "Quit" ] && break
done

# Conditional execution
[ -d /tmp ] && echo "Temp directory exists"

# Command chaining
mkdir -p ~/test && cd ~/test || echo "Failed to create/enter directory"

# Negation
! grep -q "error" logfile.txt && echo "No errors found"

# Sequential commands
echo "Step 1"; sleep 1; echo "Step 2"; sleep 1; echo "Step 3"

# Evaluating a string as code
eval "x=10; echo \$x"

# Function with local variables
calculate_sum() {
  local a=$1
  local b=$2
  echo $((a + b))
}
result=$(calculate_sum 5 3)  # result will be 8
```

## Miscellaneous

```bash
tempfile=$(mktemp)           # Creates temp file: /tmp/tmp.6tPIBs23rf
printf "Name: %s, Age: %d\n" "John" 25  # Formatted output: Name: John, Age: 25
id                           # Shows current user/group IDs
groups                       # Lists groups current user belongs to
who                          # Shows who is logged in
date "+%Y-%m-%d %H:%M:%S"    # Prints formatted date: 2025-04-06 14:30:25
time find / -name "*.txt"    # Times how long the find command takes
watch -n 1 "ls -l"           # Runs ls -l every second
tar -czvf archive.tar.gz dir/  # Creates compressed archive of directory
zip -r backup.zip documents/   # Creates zip archive of documents directory
whatis ls                    # Shows one-line description of ls command
whereis python               # Shows locations of python binary, source, manual
perl -e 'print "Hello, world\n"'  # Runs a one-liner Perl script
python -c "print('Hello')"   # Runs a one-liner Python script
less large_file.txt          # Views large_file.txt with paging
nano new_file.txt            # Edits new_file.txt with simple editor
vim script.sh                # Edits script.sh with vim editor
ack "TODO" --python          # Searches for "TODO" in Python files
grep -r "password" --include="*.conf" /etc  # Recursively searches for "password" in conf files
cal                          # Shows calendar for current month
mutt -s "Subject" recipient@example.com < message.txt  # Sends email
rsync -avz source/ dest/     # Synchronizes directories with compression
screen -S mysession          # Starts a new screen session named "mysession"
xdg-open document.pdf        # Opens PDF with default program
```

## Streams (text processing)

```bash
cat file.txt                   # Prints entire content of file.txt
head -n 5 file.txt             # Shows first 5 lines of file.txt
tail -f logfile                # Shows and follows end of logfile (updates live)
awk '{print $1, $3}' data.txt  # Prints 1st and 3rd columns from data.txt
grep "error" logfile           # Shows lines containing "error"
sed 's/old/new/g' file.txt     # Replaces all "old" with "new" in file.txt
tr '[:lower:]' '[:upper:]' < file.txt  # Converts lowercase to uppercase
shuf lines.txt                 # Outputs lines in random order
sort -k 2 -n data.txt          # Sorts data.txt by second column numerically
uniq -c counts.txt             # Shows counts of repeated lines
seq 1 5                        # Generates numbers from 1 to 5
wc -l file.txt                 # Counts lines in file.txt
column -t data.txt             # Formats data.txt in aligned columns
cut -d, -f1,3 data.csv         # Extracts columns 1 and 3 from CSV file
paste file1 file2              # Merges lines from file1 and file2 side by side
cat file.txt | tee copy.txt    # Writes to copy.txt and also to screen
```