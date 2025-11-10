◀️ [Home](../README.md)

# **CLI Essentials**
🛠 Text-based interface used to interact with a computer by entering commands. It provides direct access to system functions and scripting capabilities, often used for automation and troubleshooting.

## Table of Contents

- [`~`](#~)
- [`;`](#;)
- [`${...}`](#${...})
- [`&&`](#&&)
- [`$_`](#$_)
- [`cat`](#cat)
- [`chmod`](#chmod)
- [Conditional Blocks](#conditional-blocks)
- [`cp`](#cp)
- [`curl`](#curl)
- [`echo`](#echo)
- [`export`](#export)
- [Flags](#flags)
- [`grep`](#grep)
- [`htop`](#htop)
- [`kill`](#kill)
- [`ln`](#ln)
- [`mkdir`](#mkdir)
- [`mv`](#mv)
- [`nano`](#nano)
- [`ping`](#ping)
- [`rm`](#rm)
- [`touch`](#touch)
- [`unzip`](#unzip)

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

## `${...}`
This syntax is used for referencing a shell variable.
```bash
WORKFLOW_TRIGGER_SA="my-service-account@project.iam.gserviceaccount.com"
echo ${WORKFLOW_TRIGGER_SA}
# Output: my-service-account@project.iam.gserviceaccount.com
```
- If WORKFLOW_TRIGGER_SA is set, the shell will replace ${WORKFLOW_TRIGGER_SA} with its value.
- If it's not set, it will return an empty string (unless a default value is specified using ${VAR:-default} syntax).

## `&&` (Logical AND operator)
`&&` is a logical AND operator in Bash that runs the second command only if the first command succeeds.

```bash
mkdir my_folder && cd my_folder  
```
Creates `my_folder` and changes into it only if `mkdir` succeeds.

## `$_`
`$_` is a special variable in Bash that holds the last argument of the previous command.
```bash
mkdir my_folder && cd $_
```
`$_` expands to `my_folder`, so this is equivalent to `cd my_folder`.

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

## `chmod`
Linux command used to change file permissions (read, write, execute).

Basic syntax:
```bash
chmod [who][+/-][permission] file
```
- who → Whose permission to change:
    - u = user (owner)
    - g = group
    - o = others
    - a = all
- + / - / = → Add, remove, or set permission.
- permission
    - r = read
    - w = write
    - x = execute

Examples:
```bash
chmod u+x script.sh   # Give owner execute permission
chmod g-w file.txt    # Remove write permission for group
chmod a+r notes.txt   # Allow everyone to read
```

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

## `cp`
The `cp` (copy) command in Unix-like systems is used to copy files and directories. The syntax varies slightly depending on whether you're using it for local files (`cp`) or cloud storage (gcloud storage `cp`).

### Local File Copy
```bash
cp [options] source destination
```

#### Examples

Copy a file:
```bash
cp file1.txt file2.txt  # Copies file1.txt to file2.txt
```
Copy a file to a directory:
```bash
cp file1.txt ~/Documents/  # Copies file1.txt into Documents
```
Copy a directory (recursively):
```bash
cp -r my_folder backup_folder  # Copies my_folder and its contents
```
Common Options:
- -r → Recursive (used for directories)
- -v → Verbose (shows progress)
- -i → Interactive (asks before overwriting)
- -u → Updates only if the source is newer

## `curl`
The `curl` command is a command-line tool used to transfer data to or from a server using various network protocols, including HTTP, HTTPS, FTP, and others. It stands for **Client URL** and is commonly used for interacting with APIs, downloading or uploading files, and testing network connections.

### Common Use Cases
1. Fetch a webpage or file:

```bash
curl http://example.com
```
This will retrieve the content of `http://example.com`.

2. Download a file:

```bash
curl -O https://example.com/file.zip
```
This downloads a file and saves it with the same name as on the server (`file.zip`).

3. Send data with POST request:

```bash
curl -X POST -d "name=John&age=30" http://example.com/api
```
Sends a POST request with form data (`name=John&age=30`).

4. Include headers in the request:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" http://example.com/api
```
Sends an HTTP request with a custom header (e.g., an Authorization token).

5. Save the output to a file:

```bash
curl -o filename.txt http://example.com
```
This downloads content from the URL and saves it as `filename.txt`.

6. Show only the HTTP response headers:

```bash
curl -I http://example.com
```
This fetches only the HTTP headers (e.g., status code, content type) without the body.

### Key Options:
- `-X` : Specifies the HTTP method (e.g., GET, POST, PUT).
- `-d` : Sends data with a POST request.
- `-O` : Downloads a file and saves it with its original filename.
- `-H` : Adds custom headers to the request.
- `-I` : Fetches the response headers only.
- `-o` : Saves the output to a file.

## `echo`
The `echo` command prints text to the terminal or outputs it to a file. It's commonly used to display messages or write text into files.

```bash
echo "Hello, world"       # prints "Hello, world" to the terminal
echo "export VAR=value" >> ~/.bashrc   # appends the line to the end of the ~/.bashrc file
```

The `>>` operator in `echo` commands is used to append text to a file.
- `echo '...'`: This prints the string inside the quotes.
- `>> ~/.bashrc`: This appends the output of the `echo` command to the end of the `~/.bashrc` file, which is your Bash shell’s configuration file.

> If you used a single `>` instead of `>>`, it would overwrite the entire file — which is typically not what you want when modifying config files.

## `export`

Makes the variable available to child processes (subprocesses) started from that shell session.
```bash
export IMAGE_NAME=neon.jpg
```
Defines IMAGE_NAME as neon.jpg, which is the filename of the image being uploaded.

## Flags

- -d → Checks if it's a directory
- -e → Checks if the file exists (regardless of type)
- -f → Checks if the file exists and is a regular file (not a directory).
- -n → Checks if a string is not empty
- -r → Checks if the file is readable
- -w → Checks if the file is writable
- -x → Checks if the file is executable

<br>

- --user
```bash
pip3 install -r ~/bookshelf/requirements.txt --user
```
Installs the packages for the current user only instead of system-wide.

Why use --user?

    - Avoids needing admin/root privileges (no sudo required).
    - Installs packages in ~/.local/ (Linux/macOS) or %APPDATA%\Python (Windows).
    - Prevents conflicts with system Python packages.

When to use it?

    - When you don’t have admin rights.
    - When you want to keep system Python clean.
    - If using a single-user setup without a virtual environment.
> Using virtual environments (venv or conda) is usually preferred over --user for better package management.

## `grep`

Searches for patterns in text.

```bash
grep -r "pattern" [directory]
```

### Useful Options

#### `-r` (or `--recursive`) → means search in all files in the current directory and all subdirectories

```bash
grep -r "pattern" [directory]
```

#### `-i` → ignore case

```bash
grep -ri "error" .
```

#### `-n` → show line numbers

```bash
grep -rn "main()" src/
```

#### `--color=auto` → highlight matches (often on by default)

#### `--include` → only search specific file types

```bash
grep -r "import" --include="*.py" .
```

#### `--exclude-dir` → skip certain directories

```bash
grep -r "password" . --exclude-dir={.git,node_modules}
```

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

## `ln`
Creates links between files or directories.
- Soft links act like shortcuts; they depend on the original file.
- Hard links are alternative names for the same data and remain functional even if the original file is deleted.

| Feature                         | **Soft Link (Symbolic Link)**          | **Hard Link**                           |
|---------------------------------|--------------------------------|--------------------------------|
| **Definition**                  | A pointer to the original file. | A duplicate reference to the same file data on disk. |
| **Inode** `[^1]`          | Has a different inode number than the original file. | Shares the same inode as the original file. |
| **Works Across Filesystems?**    | Yes, can link to a file on a different filesystem. | No, must be on the same filesystem. |
| **Works for Directories?**       | Yes, can link to directories. | No, hard links cannot be created for directories. |
| **If Original File is Deleted?** | The soft link breaks (becomes useless). | The hard link still works because the file data remains. |
| **Command to Create**            | `ln -s target link_name` | `ln target link_name` |

`[^1]`: An inode (index node) is a data structure used by Unix-like file systems to store metadata about a file. It contains information such as: File size, File type (regular file, directory, etc.), Permissions (read, write, execute), Owner and group ID, Timestamps (creation, modification, access), Pointers to data blocks (where the file's actual content is stored). Each file has a unique inode number, except hard links, which share the same inode. Soft (symbolic) links, however, have different inodes. To check a file's inode number, use: `ls -i filename`.

> *What is the difference between a hard link and copying the file?* <br>
> A hard link is just another name for the same file, pointing to the same data on disk. A copied file is a completely independent version, taking up extra space.

The following command creates a symbolic link (~/code) that points to ~/training-data-analyst/courses/orchestration-and-choreography/lab1. After running this command, instead of navigating to the full path, you can just use ~/code to access the same location.
```bash
ln -s ~/training-data-analyst/courses/orchestration-and-choreography/lab1 ~/code
```
- `-s` → Creates a symbolic link (soft link) instead of a hard link.
- `~/training-data-analyst/courses/orchestration-and-choreography/lab1` → The target directory you want to link to.
- `~/code` → The name/location of the symbolic link being created.

## `mkdir`

Command used to create new directories (folders) in a filesystem.

```bash
mkdir my_folder  # Creates a directory named "my_folder"
```
Use -p to create parent directories if they don’t exist:
```bash
mkdir -p parent/child  # Creates "parent" and "child" inside it if they don't exist
```

## `mv`
The `mv` command in Unix/Linux is used to move or rename files and directories.

Move a file:
```bash
mv file.txt /path/to/destination/
```
Rename a file:
```bash
mv oldname.txt newname.txt
```
Move and rename at the same time:
```bash
mv file.txt /new/path/newname.txt
```

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

## `rm`

Used to delete files or directories.

### Useful Options

- `-r` → recursive (tells `rm` to delete directories and their contents, including all subdirectories and files)
- `-f` → force (tells `rm` to ignore nonexistent files and not prompt for confirmation, even if files are write-protected)

> `rm -rf [path]` means “delete everything inside this path, no questions asked.”

## `touch`
Creates an empty file or updates the timestamp of an existing file.

```bash
touch file.txt
```
Creates `file.txt` if it doesn’t exist or updates its last modified time if it does.

## `unzip`
Extracts files from a `.zip` archive.

```bash
unzip archive.zip
```
Extracts `archive.zip` contents into the current directory.