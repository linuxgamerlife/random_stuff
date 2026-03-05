# Fedora Linux Terminal Cheat Sheet (Beginner Friendly)

This cheat sheet covers the most useful terminal commands for **Fedora Linux**, aimed at beginners.

You do not need to memorise everything. Use this as a quick reference.

## Navigation

| Command | What it does |
|--------|--------------|
| `pwd` | Shows your current location in the filesystem |
| `ls` | Lists files and folders in the current directory |
| `ls -l` | Lists files with details (permissions, owner, size, date) |
| `ls -a` | Shows hidden files (names starting with `.`) |
| `ls -lah` | Detailed list, includes hidden files, human readable sizes |
| `cd foldername` | Moves into a folder |
| `cd ..` | Moves up one directory |
| `cd -` | Jumps back to the previous directory |
| `cd ~` | Returns to your home directory |

## File and Folder Management

| Command | What it does |
|--------|--------------|
| `mkdir newfolder` | Creates a new folder |
| `mkdir -p a/b/c` | Creates nested folders in one go |
| `touch file.txt` | Creates an empty file |
| `cp file.txt backup.txt` | Copies a file |
| `cp -r folder1 folder2` | Copies a folder and its contents |
| `mv oldname.txt newname.txt` | Renames or moves a file |
| `rm file.txt` | Deletes a file |
| `rm -r foldername` | Deletes a folder and everything inside it |
| `rmdir foldername` | Deletes an empty folder only |

Note: Use `rm` carefully. Files deleted this way are usually not recoverable.

## Viewing Files

| Command | What it does |
|--------|--------------|
| `cat file.txt` | Displays the contents of a file |
| `less file.txt` | Opens a file with scrolling. Press `Q` to exit |
| `more file.txt` | Opens a file page by page (simpler than `less`) |
| `head file.txt` | Shows the first lines of a file |
| `head -n 20 file.txt` | Shows the first 20 lines |
| `tail file.txt` | Shows the last lines of a file |
| `tail -n 50 file.txt` | Shows the last 50 lines |
| `tail -f logfile.txt` | Follows a file as it updates (useful for logs) |

## Editing Files (Beginner Friendly)

| Command | What it does |
|--------|--------------|
| `kwrite file.txt` | Opens a file in KWrite (great for KDE users) |
| `nano file.txt` | Opens a file in Nano (terminal editor) |
| `vi file.txt` | Opens Vim (powerful, not beginner friendly) |

## Help and Documentation

| Command | What it does |
|--------|--------------|
| `man ls` | Shows the manual page for a command |
| `command --help` | Quick help for a command |
| `tldr ls` | Simple examples for a command (if `tldr` is installed) |

## Searching and Finding Things

| Command | What it does |
|--------|--------------|
| `find . -name file.txt` | Finds a file in the current folder and subfolders |
| `grep "text" file.txt` | Searches for text inside a file |
| `grep -n "text" file.txt` | Shows matching lines and line numbers |
| `grep -R "text" .` | Searches text in all files under the current folder |

Example:  
`grep -n "error" logfile.txt`

## System Information

| Command | What it does |
|--------|--------------|
| `whoami` | Shows the current user |
| `hostname` | Shows the system name |
| `uname -a` | Shows system and kernel information |
| `uptime` | Shows how long the system has been running |
| `df -h` | Shows disk space usage |
| `du -h -d 1` | Shows folder sizes one level deep |
| `free -h` | Shows memory usage |
| `lsblk` | Shows drives and partitions |
| `ip a` | Shows network interfaces and IP addresses |

## Processes and Performance

| Command | What it does |
|--------|--------------|
| `top` | Shows running processes and system load |
| `htop` | Better version of `top` (if installed) |
| `ps aux` | Lists running processes |
| `kill PID` | Stops a process by PID |
| `killall appname` | Stops processes by name |

## Software Management (DNF)

Fedora uses **DNF** to install and manage software.

| Command | What it does |
|--------|--------------|
| `sudo dnf install steam` | Installs software |
| `sudo dnf update` | Updates all packages |
| `sudo dnf remove steam` | Removes software |
| `dnf search steam` | Searches for packages |
| `dnf info steam` | Shows package details |
| `dnf list installed` | Lists installed packages |
| `dnf history` | Shows install and update history |
| `sudo dnf autoremove` | Removes orphaned dependencies |

## Flatpak

| Command | What it does |
|--------|--------------|
| `flatpak list` | Lists installed Flatpak apps |
| `flatpak search appname` | Searches for Flatpak apps |
| `flatpak install flathub app.id` | Installs a Flatpak app |
| `flatpak update` | Updates all Flatpak apps |
| `flatpak uninstall app.id` | Uninstalls a Flatpak app |

## Permissions

| Command | What it does |
|--------|--------------|
| `sudo command` | Runs a command as administrator |
| `chmod +x script.sh` | Makes a script executable |
| `ls -l` | Shows file permissions |

## Archives (Zip and Tar)

| Command | What it does |
|--------|--------------|
| `tar -xf file.tar` | Extracts a tar archive |
| `tar -xzf file.tar.gz` | Extracts a gzipped tar archive |
| `zip -r archive.zip folder` | Creates a zip file from a folder |
| `unzip archive.zip` | Extracts a zip file |

## Downloads

| Command | What it does |
|--------|--------------|
| `curl -O https://example.com/file` | Downloads a file |
| `wget https://example.com/file` | Downloads a file (if installed) |

## System Power Commands

| Command | What it does |
|--------|--------------|
| `sudo reboot` | Restarts the system |
| `sudo shutdown now` | Shuts down immediately |
| `sudo poweroff` | Powers the system off |

## Terminal Utilities

| Command | What it does |
|--------|--------------|
| `clear` | Clears the terminal screen |
| `history` | Shows recent commands |
| `history | tail` | Shows the last commands |
| `!!` | Repeats the last command |
| `sudo !!` | Repeats the last command with sudo |

## Useful Keyboard Shortcuts

| Shortcut | Action |
|--------|--------|
| `Ctrl + C` | Stops a running command |
| `Ctrl + L` | Clears the terminal |
| `Ctrl + D` | Logs out of the terminal |
| `Tab` | Auto completes commands or filenames |
| `Arrow Up` | Shows previous commands |
| `Ctrl + R` | Search command history (type then press Enter) |

## Final Thoughts

Start with `pwd`, `ls`, `cd`, and `dnf`. Once those feel natural, the rest becomes much easier.
