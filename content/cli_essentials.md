◀️ [Home](../README.md)

# **CLI Essentials**
🛠 Text-based interface used to interact with a computer by entering commands. It provides direct access to system functions and scripting capabilities, often used for automation and troubleshooting.

## Table of Contents

- [`~`](#~)
- [`;`](#;)
- [`cat`](#cat)
- [Conditional Blocks](#conditional-blocks)
- [Flags](#flags)
- [`htop`](#htop)
- [`kill`](#kill)
- [`nano`](#nano)
- [`ping`](#ping)

## `~`
The `~` (tilde) represents the home directory of the current user in Unix-based systems like macOS and Linux.

If your username is `pablogarcia`, then `~` is equivalent to `/Users/pablogarcia/` on macOS. If you were on Linux, `~` would typically be `/home/pablogarcia/`.

- `cd ~` → Moves to your home directory (`/Users/pablogarcia/`).
- `ls ~/Downloads` → Lists files inside your Downloads folder.
- `~/scripts/myscript.sh` → Runs `myscript.sh` inside the scripts folder in your home directory.

## `;`
The `;` in the command line is used to separate multiple commands so they run sequentially on the same line. 

```sh
cd ~/bookshelf; ~/.local/bin/gunicorn -b :8080 main:app
```
- `cd ~/bookshelf` → Changes the directory to bookshelf
- `;` → Separates the two commands
- `~/.local/bin/gunicorn -b :8080 main:app` → Runs the Gunicorn server

## `cat`
The `cat` command in Unix-based systems (like macOS and Linux) is used to concatenate and display the contents of files. It is mostly used to quickly view or combine files.

### Common Uses
1. View a file’s contents
```bash
cat filename.txt
```
> *Prints the content of filename.txt to the terminal.*

2. Concatenate multiple files
```bash
cat file1.txt file2.txt > combined.txt
```
> *Merges file1.txt and file2.txt into combined.txt.*

3. Create a new file
```bash
cat > newfile.txt
```
> *Starts writing into newfile.txt (Press Ctrl + D to save).*

```sh
cat > ~/bookshelf/oauth.py <<EOF
# (Python code here)
EOF
```
> 1. *Creates (or overwrites) a file named oauth.py inside the bookshelf directory.*
> 2. *Writes the Python code inside the file (between <<EOF and EOF).*
> 3. *Saves and closes the file automatically.*

4. Append content to an existing file
```bash
cat file1.txt >> file2.txt
```
> *Appends the content of file1.txt to file2.txt.*

5. Number the lines in a file
```bash
cat -n filename.txt
```
> *Displays filename.txt with line numbers.*

## Conditional Blocks

```bash
if [[ -f myfile.txt ]]; then
    echo "File exists"
else
    echo "File does not exist"
fi  # End of if statement
```

- `if ...; then` → Starts the conditional block.
- `else` (optional) → Specifies an alternative block.
- `fi` → Closes the if statement. It's just "if" spelled backward—a common pattern in shell scripting.

## Flags

-d → Checks if it's a directory
-e → Checks if the file exists (regardless of type)
-f → Checks if the file exists and is a regular file (not a directory).
-n → Checks if a string is not empty
-r → Checks if the file is readable
-w → Checks if the file is writable
-x → Checks if the file is executable

## `htop`
The htop command is an interactive process viewer for Unix-based systems. It allows users to monitor system resources (CPU, memory, etc.) and manage processes in real time, with features like sorting, filtering, and killing processes directly from the interface.
```bash
htop
```

## `kill`
Use the kill command followed by the process ID (PID) 
```bash
kill -9 <PID>
```

```bash
ps aux | grep <[app.py](http://app.py/)>
```
> *Use the ps command to locate the process associated with your application or Python script*

### Example

```bash
(quality_checker) pablogarcia@Pablos-MacBook-Pro cap_windows % ps aux | grep [app.py](http://app.py/)
pablogarcia        468   0.1  0.5 413903088 179056   ??  S     1:05PM   0:17.97 /opt/homebrew/Caskroom/miniconda/base/envs/quality_checker/bin/python [app.py](http://app.py/)
pablogarcia        455   0.0  0.2 412976304  69216   ??  S     1:05PM   0:01.41 python [app.py](http://app.py/)
pablogarcia       2405   0.0  0.0 410063504    528 s014  R+    1:29PM   0:00.00 grep [app.py](http://app.py/)
```
> *The PID (Process ID) is the number in the second column of the output from the ps aux command. It’s a unique identifier assigned to each running process.*

```bash
kill -9 468 455
```
> *To stop the Python script, we want to terminate the processes with PIDs 468 and 455 (related to app.py)*

## `nano`
The `nano` command is used to open the Nano text editor, a simple and user-friendly command-line text editor available on Unix-based systems like Linux and macOS.
```sh
nano myfile.txt
```
> *Opens (or creates) myfile.txt for editing in the terminal.*

### Common Nano Commands
| Shortcut  | Action          |
|-----------|----------------|
| CTRL + O  | Save (Write Out) |
| CTRL + X  | Exit            |
| CTRL + K  | Cut a line      |
| CTRL + U  | Paste a line    |
| CTRL + W  | Search          |
| CTRL + G  | Show help       |

> *After `CTRL + X`, you will have to type `Y` and press `Enter` to save the file.*

## `ping`
The ping command is used to check the reachability of a host on a network and measure round-trip time for data packets.
```bash
ping [options] <hostname or IP address>
```
For example:
```bash
ping google.com
```

### Common Options
- -c [count]: Set number of pings to send.
```bash
ping -c 5 google.com
```
- -t [time]: Set timeout for replies.
- -i [interval]: Set interval between pings.
- -s [size]: Set packet size.

### Output
> - Reply from [IP address]: The destination is reachable.
> - time: Round-trip time in milliseconds (ms).
> - TTL (Time to Live): Number of hops the packet made.