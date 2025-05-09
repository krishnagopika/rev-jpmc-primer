
### Unix Permissions

#### **Users and Files**  

```bash

# create user
sudo useradd username

# change the password
sudo passwd username

# switch user
su - username

```
- Unix permissions revolve around three user types: **Owner**, **Group**, and **Others**.  
- Permissions include **read (r)**, **write (w)**, and **execute (x)** for each user type.  
- Use `chmod` to modify permissions, `chown` to change file ownership, and `chgrp` to change group ownership.  

#### **Files**  
- Regular files, directories, symbolic links, etc., all have specific permission settings.  
- **`ls -l`** shows detailed permissions in the format: `-rwxr-xr--` (Owner, Group, Others).  


**References:**

1. [redhat system administrator](https://www.redhat.com/en/services/training/rh124-red-hat-system-administration-i)
2. [file permissions](https://www.redhat.com/en/blog/linux-file-permissions-explained)
3. [file navitagion](https://www.redhat.com/en/blog/Linux-file-navigation-commands)

### Find Command

- Locates files and directories based on conditions like name, size, type, or permissions.  
- Example: `find /path -name "*.txt" -exec rm {} \;` finds and deletes all `.txt` files.  
- Super versatile and supports actions like deleting, copying, or executing commands.  

[find](https://www.redhat.com/en/blog/linux-find-command)


```bash
find ./ -type f -newermt "2024-12-06" ! -newermt "2024-12-07"


find ./ -type f -name "demo.txt" -newermt "2024-12-06" ! -newermt "2024-12-07" -exec cp {} ./copy_dir \;
```

---

### Cut Command  
- Used to extract sections of text or fields from files or command output.  
- Example: `cut -d ',' -f 2 file.csv` extracts the second field from a CSV file.  
- Works well with delimiters like tabs, spaces, or custom characters.  

[cut](https://www.baeldung.com/linux/cut-command)

---

### AWK Command  
- A text-processing tool that works with pattern matching and fields.  
- Example: `awk '{print $1}' file.txt` prints the first column of each line.  
- Powerful for data extraction, transformation, and analysis.  

[awk]([https://](https://www.freecodecamp.org/news/the-linux-awk-command-linux-and-unix-usage-syntax-examples/))
---


```bash
 awk -F ':'  '{print $1 $4}'  passwd
 awk -F ':'  'BEGIN{OFS=" - "} {print $1,$4}'  passwd
 awk -F ':'  'BEGIN{OFS=" - "} {print $1,$4,$NF}'  passwd
```

note: $0 - everything $1 - 1st coloumn and $NF - last coloumn

### Unix Sort Command  
- Sorts lines of text files alphabetically, numerically, or by fields.  
- Example: `sort -n file.txt` sorts numerically, while `sort -r` sorts in reverse.

[sort](https://www.redhat.com/en/blog/sort-command-linux)

```bash

# text asc
sort filename

# text desc

sort -r filename

# numbers 

sort -n filename

# unique values

sort -u filename

# months

sort -M filename
```
---

### Tar Command  

#### **Introduction to `tar`**
- **Purpose**: Short for "tape archive," `tar` is used for archiving multiple files/directories into a single file, often for backups or transfers.
- **Key Features**:  
  - Combines files into a `.tar` archive.  
  - Optionally compresses the archive using tools like gzip (`.tar.gz`) or bzip2 (`.tar.bz2`).

---

#### **Basic Syntax**
```bash
tar [OPTIONS] [ARCHIVE_NAME] [FILES/DIRECTORIES]
```
- **Options**: Control the behavior (e.g., create, extract, compress).  
- **Archive Name**: Specifies the file to be created or extracted.  
- **Files/Directories**: Lists the items to include.

---

#### **Key Operations**
1. **Create an Archive**:
   ```bash
   tar -cvf archive.tar file1 file2 dir/
   ```
   - **`-c`**: Create an archive.  
   - **`-v`**: Verbose (lists files being added).  
   - **`-f`**: Specify the archive file name.

2. **Extract an Archive**:
   ```bash
   tar -xvf archive.tar
   ```
   - **`-x`**: Extract files from an archive.

3. **View Contents**:
   ```bash
   tar -tvf archive.tar
   ```
   - **`-t`**: List files in the archive.

---

#### **Adding Compression**
1. **Gzip Compression**:
   ```bash
   tar -czvf archive.tar.gz file1 dir/
   ```
   - **`-z`**: Use gzip compression.

2. **Bzip2 Compression**:
   ```bash
   tar -cjvf archive.tar.bz2 file1 dir/
   ```
   - **`-j`**: Use bzip2 compression.

3. **Extracting Compressed Archives**:
   ```bash
   tar -xzvf archive.tar.gz
   tar -xjvf archive.tar.bz2
   ```

---

#### **Advanced Features**
1. **Exclude Files**:
   ```bash
   tar -cvf archive.tar --exclude="*.log" dir/
   ```
   - Skips files matching the pattern.

2. **Incremental Backups**:
   - Use snapshots to back up only changed files:
     ```bash
     tar --listed-incremental=snapshot.file -cvf backup.tar dir/
     ```

3. **Appending to an Archive**:
   ```bash
   tar -rvf archive.tar newfile
   ```
   - **`-r`**: Append files to an existing archive.

4. **Verify Archives**:
   ```bash
   tar -tvf archive.tar
   ```

---

#### **Real-World Use Cases**
- **Backups**: Create backups of directories.
- **Deployments**: Package software or configurations.
- **File Transfers**: Bundle and compress files for easy transfer.

---

#### **Common Pitfalls**
1. Forgetting the `-f` option when specifying an archive name.
2. Incorrect file paths or permissions during extraction.
3. Large files might require split archives:
   ```bash
   tar -cvf - largefile | split -b 100M - part-
   ```

---

#### **Best Practices**
1. Always verify archives (`-tvf`) after creation.
2. Use compression for space efficiency.
3. Automate repetitive tasks using shell scripts.


[tar](https://www.redhat.com/en/blog/taming-tar-command)

---

### Process Control Commands  

#### **Process Management Shortcuts**:  
- **`Ctrl + C`**: Terminates the running process.  
- **`Ctrl + Z`**: Suspends a running process.  

#### `jobs`  
- Lists background and suspended jobs.  

#### `fg`  
- Brings a background job to the foreground.  

```bash
fg %1
```

#### `bg`  
- Resumes a suspended job in the background. 

```bash
bg %1
``` 

#### `&`  
- Appends to run a command in the background. Example: `command &`. 


```bash
ping google.com &
```

#### `nohup`  
- Runs a command immune to hangups (terminal closure). Example: `nohup command &`.  

### `ps aux | grep process`

- A command to search the process by its name.

#### `kill`  
- Sends a signal to terminate a process by PID. Example: `kill 1234`.  

#### `killall`  
- Terminates all processes matching a name. Example: `killall ping`.  

#### `pkill`  
- Kills processes matching a regex pattern. Example: `pkill -9 sshd`.  

#### Signals:  
- `kill -15`: Graceful stop (default).  SIGTERM 
- `kill -9`: Forceful stop.  
- `kill -1`: Reloads process configuration.  

---

### Process Monitoring Commands  

#### `ps`  
- Displays currently running processes. Example: `ps -e`.  

#### `ps -ef`  
- Extended full-format listing of all processes.  

#### `pgrep`  
- Finds PIDs matching a pattern. Example: `pgrep nginx`.  

#### `top`  
- Interactive real-time process viewer.  

#### `free`  
- Displays system memory usage.  

#### `uptime`  
- Shows how long the system has been running.  

#### `watch`  
- Repeats a command every X seconds. Example: `watch -n 2 df -h`. 


---

## grep

global regular expression print.


```bash
# get lines with name
grep name filename

#get lines that doesnt contain the name
grep -v name filename

# line numbers
grep -n name filename

# count
grep -c name filename

# ignore case

grep -i name filename

# list of files that contain the name

grep name *

# list of files that contain the name along with line numbers

grep -n name *

# recursively search through diretives

grep -r name path/to/dir

```


### regex

```
^      Matches characters at the beginning of a line
$      Matches characters at the end of a line
"."    Matches any character
"*"    Matches 0 or more of the preceding element
"+"    Matches 1 or more of the preceding element
"?"    Matches 0 or one occurance
[a-z]  Matches any characters between A and Z
[^ ..] Matches anything apart from what is contained in the brackets
```


```bash

# starting with b

"^b"

# ends with y

"y$"

# mathces c_e

"c.e"

# matches apXnle 

"ap*le"

# character set

"[aeiou]"

# negate

"[^aeiou]"

# or

"apple\|bleuberry"

# escaping meta characters - lines containing space

"\ "

# quntifier 2 

"r\{2\}"

# between 1 and 3

"a\{1,3\}"

# 2 vowels

"{aeiou}\{2\}"


# get sesssion

"session"

# sudo commands for specific users

"sudo:.*user"

```


## VIM



```
vimtutor
```

- Just type vimtutor in your terminal and press Enter. This will launch an interactive tutorial that walks you through the basics of using Vim.

### **Key `vim` Commands**


#### Basic Modes

- i → Insert mode (type text)
- Esc → Normal mode (navigate & command)
- : → Command mode (enter commands like :w, :q)

# Basic Editing

- i → Insert before cursor
- I → Insert at beginning of line
- a → Append after cursor
- A → Append at end of line
- o → Open new line below
- O → Open new line above

#### Saving and Quitting

```
:w → Save (write)

:q → Quit

:wq or ZZ → Save and quit

:q! → Quit without saving

```

#### Navigation

```
h → Left

l → Right

j → Down

k → Up

w → Next word

b → Previous word

0 → Start of line

^ → First non-blank character

$ → End of line

gg → Go to start of file

G → Go to end of file

:n → Go to line n

```

#### **1. Copy, Cut, and Paste**

- **Copy a line**:  
  `yy` (yank the current line)  
  Example: Place the cursor on the line and press `yy`. Use `p` to paste it below or `P` to paste it above.

- **Copy multiple lines**:  
  `3yy` (copy 3 lines starting from the current line).  

- **Cut a line**:  
  `dd` (delete the current line and save it to the clipboard).  
  Example: Cut the current line with `dd` and paste it with `p`.

- **Cut multiple lines**:  
  `3dd` (cut 3 lines starting from the current line).  

- **Copy/cut a word**:  
  `yw` (yank a word), `dw` (cut/delete a word).  

---

#### **2. Searching**
- **Search forward**:  
  `/pattern`  
  Example: `/error` to search for "error" in the file.

- **Search backward**:  
  `?pattern`  
  Example: `?debug` to search for "debug" in reverse.

- **Next/previous match**:  
  `n` (next match), `N` (previous match).

---

#### **3. Undo and Redo**
- **Undo**:  
  `u`  
  Example: Undo the last action.

- **Redo**:  
  `Ctrl + r`  
  Example: Redo the undone action.

---

#### **4. Saving and Exiting**
- **Save**:  
  `:w`  
  Example: Save the current file.

- **Save and exit**:  
  `:wq` or `ZZ`.  

- **Exit without saving**:  
  `:q!`  

---

#### **5. Navigation**
- **Move to the beginning of the file**:  
  `gg`  

- **Move to the end of the file**:  
  `G`  

- **Move to a specific line**:  
  `:number`  
  Example: `:42` moves to line 42.

- **Navigate words**:  
  `w` (forward one word), `b` (backward one word).  

---

#### **6. Line Operations**
- **Delete a line**:  
  `dd`  

- **Delete from the cursor to the end of the line**:  
  `d$`  

- **Join lines**:  
  `J`  
  Example: Joins the current line with the next line.

#### **7. Miscellaneous**
- **Enable line numbers**:  
  `:set number`  

- **Disable line numbers**:  
  `:set nonumber`  

- **Find and replace**:  
  `:%s/old/new/g`  
  Example: `:%s/debug/log/g` replaces "debug" with "log" globally in the file.

- **Open a new file in the current session**:  
  `:e filename`  

- **Switch between open files**:  
  `:bn` (next buffer), `:bp` (previous buffer).  


### **Example Use Cases**

1. **Search for all errors in the log file**:
   - Command: `/ERROR`  
   - Use `n` to jump to the next occurrence and `N` for the previous.

2. **Delete lines containing debug messages**:
   - Command: `:g/DEBUG/d`

3. **Find and replace "ERROR" with "FAILURE"**:
   - Command: `:%s/ERROR/FAILURE/g`

4. **Copy the first 5 lines to the end of the file**:
   - Command: `5yy` to yank 5 lines, then `G` and `p` to paste at the end.

5. **Enable line numbers for better navigation**:
   - Command: `:set number`

---


# Shell Scripting Notes

## Shell
- A shell is an interface to the OS that allows users to execute commands.
- Common shells include Bash, C Shell, Korn Shell, and Z Shell.
- To check the current shell:

```sh
echo $0
```

- Shell scripts are files containing a series of commands.
- The shell interprets and executes the script.
- Useful for automating tasks such as system monitoring, log file analysis, and backup management.

### Shebang
- Specifies the interpreter for the script.
- Recommended but not mandatory.

```sh
#!/bin/bash
```

## I/O Standard Input, Standard Output, and Standard Error
- **Standard Input (stdin):** Data input (usually from the keyboard).
- **Standard Output (stdout):** Normal command output.
- **Standard Error (stderr):** Error messages.

Redirecting output:
```sh
ls > output.txt  # Redirect stdout to file
ls 2> error.txt  # Redirect stderr to file
ls &> all_output.txt  # Redirect both stdout and stderr to file
cat file.txt  # Read from a file and display
```

## Variables
- Assign values using `=`.
- Use `$VAR_NAME` to reference a variable.
- Enclose values in quotes if they contain spaces.

```sh
NAME="John Doe"
echo "Hello, $NAME"
```

## Special Variables
- `$0` - Script name
- `$1, $2, ...` - Positional parameters
- `$#` - Number of arguments
- `$?` - Exit status of last command
- `$$` - Process ID of script
- `$@` - All arguments

```sh
echo "Script name: $0"
echo "First argument: $1"
echo "Number of arguments: $#"
```

## Reading Input
- Read input from the user.

```sh
echo "Enter your username:"
read USERNAME
echo "Logging in as $USERNAME..."
```

## Command Substitution
- Captures command output.

```sh
CURRENT_DATE=$(date +"%Y-%m-%d %H:%M:%S")
# $() is similar to ``
CURRENT_DATE=`date +"%Y-%m-%d %H:%M:%S"`

CURRENT_TIME=`date +"%H:%M:%S"`
echo "Current timestamp: $CURRENT_DATE current time: $CURRENT_TIME"
```

## Conditionals and Operators
- **If-else statement:**

```sh
if [ "$USERNAME" == "admin" ]; then
    echo "Welcome, Admin!"
else
    echo "Access denied."
fi
```

### Operators:
- **Arithmetic Operators:** `+ - * / %`  
  ```sh
  x=10
y=5
sum=$((x + y))
echo "Sum: $sum"
  ```

- **Comparison Operators:** `-eq, -ne, -gt, -lt, -ge, -le`
  ```sh
  if [ $x -gt $y ]; then
      echo "$x is greater than $y"
  fi
  ```

- **Logical Operators:** `&&, ||`
  ```sh
  if [ $x -gt 0 ] && [ $y -gt 0 ]; then
      echo "Both numbers are positive"
  fi
  ```

## Exit Statuses and Return Codes
- `$?` stores the exit status of the last executed command (0 = success, non-zero = failure).

```sh
ping -c 1 google.com
if [ $? -eq 0 ]; then
    echo "Internet is up."
else
    echo "No Internet connection."
fi
```

## String Test Conditionals
- Checking string conditions:

-  Using the test command `([ ])`

- check if string is empty

```sh
if [ -z "$VAR" ]; then
    echo "VAR is empty"
fi
```
- check if strings are equal

```sh
if [ "$STR1" = "$STR2" ]; then
echo "STR1 and STR2 are equal"
fi
```

- contains a sub string

```sh
string="Hello World"

if [[ "$string" == *"World"* ]]; then
  echo "String contains 'World'"
fi
```

- `[[ ]]` bash specific. Check if a string macthes a regex

```sh
if [[ $STR =~ ^[a-zA-Z]+$ ]]; then
echo "STR is a string of letters"
fi
```

## Positional Parameters, Arguments, for Loops
- Using arguments:

```sh
./script.sh arg1 arg2
```

- **For loop:**

```sh
for server in server1 server2 server3; do
    ping -c 1 $server
    echo "$server checked"
done
```

## While Loop, Infinite Loops, Shifting, Sleep
- **While loop:**

```sh
COUNT=1
while [ $COUNT -le 5 ]; do
    echo "Checking server status..."
    ((COUNT++))
    sleep 5  # Pause for 5 seconds

done
```

- **Infinite loop:**

```sh
while true; do
    echo "Monitoring logs..."
    tail -f /var/log/syslog
    sleep 5
done
```

## Case Statements
- Alternative to multiple `if-elif-else` conditions.

```sh
echo "Enter a service (nginx/apache/mysql):"
read SERVICE
case $SERVICE in
    nginx) systemctl restart nginx ;;
    apache) systemctl restart apache2 ;;
    mysql) systemctl restart mysql ;;
    *) echo "Unknown service." ;;
esac
```

## Functions
- Define and call functions.

```sh
backup_logs() {
    tar -czf backup_$(date +%Y%m%d).tar.gz /var/log
    echo "Logs backed up."
}

backup_logs
```


