**Linux Commands Cheatsheet**

Every command learned in this repository in one place.
Format: command —> what it does —> example

---
**Navigation**

| Command |	What It Does | Example |
| :--- | :--- | :--- |
| `pwd` | Show current location | `pwd` |
| `ls` | List directory contents | `ls -la` |
| `cd` | Change directory | `cd /home/tuhin` |
| `cd ..` | Go one level up | `cd ..` |
| `cd ../..` | Go two level up | `cd ../..` |
| `cd ~` | Go to home directory | `cd ~` |
| `cd -` | Go to previous directory | `cd -` |
| `clear` | Clear terminal screen | `clear` |
| `history` | Show command history | `history 20` |
| `man` | Open command manual | `man ls` |

---
**File Management**

| Command |	What It Does | Example |
| :--- | :--- | :--- |
| `touch` |	Create empty file |	`touch file.txt` |
| `mkdir` |	Create directory | `mkdir -p projects/linux` |
| `cp` |	Copy file or folder | `cp -r folder/ backup/` |
| `mv` |	Move or rename | `mv old.txt new.txt` |
| `rm` |	Delete file or folder | `rm -rf folder/` |
| `rmdir` |	Delete empty directory | `rmdir empty_folder/` |
| `cat` |	Display file contents | `cat file.txt` |
| `less` |	Scroll through file	| `less largefile.txt` |
| `head` |	Show first N lines	| `head -n 20 file.txt` |
| `tail` |	Show last N lines	| `tail -f logfile.txt` |
| `find` |	Search for files	| `find / -name "*.sh"` |
| `file` |	Identify file type	| `file suspicious_file` |
| `wc` |	Count lines/words/chars	| `wc -l file.txt` |

---
**User Management**

| Command |	What It Does | Example |
| :--- | :--- | :--- |
| `whoami` | Show current user | `whoami` |
| `id` | Show user and group IDs | `id username` |
| `useradd` | Create new user | `sudo useradd -m -s /bin/bash thor` |
| `passwd` | Set user password | `sudo passwd thor` |
| `usermod` | Modify user	| `sudo usermod -aG avengers thor` |
| `userdel` | Delete user	| `sudo userdel -r thor` |
| `groupadd` | Create group	| `sudo groupadd avengers` |
| `groups` | Show group membership | `groups thor` |
| `sudo` | Run as superuser	| `sudo command` |

---
**Permissions**

| Command |	What It Does | Example |
| :--- | :--- | :--- |
| `chmod` |	Change permissions | `chmod 755 script.sh` |
| `chown` |	Change owner | `sudo chown thor:dev file.txt` |
| `ls -l`|	View permissions | `ls -la` |

**Permission numbers: r(read)=4, w(write)=2, x(execute)=1 — add for combinations**

* 7 = rwx (full)
* 6 = rw- (read & write)
* 5 = r-x (read & execute)
* 4 = r-- (read)
* 3 = -wx (write & execute)
* 2 = -w- (write)
* 1 = --x (execute)

**Fun Fact**
The numbers for read, write and execute are from binary
```text
    r   w   e
    1   0   0  ->  4 (read)
    0   1   0  ->  2 (write)
    0   0   1  ->  1 (execute)
```
---
**Process Management**

| Command |	What It Does | Example |
| :--- | :--- | :--- |
| `ps aux` | Show all processes | `ps aux` |
| `top` | Live process monitor | `top` |
| `htop` | Better live monitor | `htop` |
| `kill` | Terminate by PID | `kill -9 PID` |
| `killall` | Terminate by name | `killall nginx` |
| `pgrep` | Find PID by name | `pgrep nginx` |
| `jobs` | Show background jobs | `jobs` |
| `bg` | Send to background | `bg` |
| `fg` | Bring to foreground | `fg %1` |
| `nohup`	| Run after logout | `nohup command &` |
| `systemctl` |	Manage services | `sudo systemctl status nginx` |

---
**Text Processing**

| Command |	What It Does | Example |
| :--- | :--- | :--- |
| `grep` |	Search for pattern | `grep -i "error" log.txt` |
| `cut` |	Extract columns | `cut -d: -f1 /etc/passwd` |
| `sort` |	Sort lines | `sort -rn file.txt` |
| `uniq` |	Remove duplicates	| `sort file.txt \| uniq -c` |
| `sed` |	Find and replace | `sed 's/old/new/g' file.txt` |
| `awk` |	Process columns	| `awk '{print $1}' file.txt` |
|	`Pipe` | between commands	| `ps aux \| grep nginx` |

---
**Essential Shortcuts**

| Shortcut | What It Does |
| :--- | :--- |
| Ctrl + C | Kill running command |
| Ctrl + Z | Pause running command |
| Ctrl + L | Clear screen |
| Ctrl + A | Go to start of line |
| Ctrl + E | Go to end of line |
| Tab |	Autocomplete |
| ↑ ↓ |	Navigate command history |

---
This cheatsheet grows with every topic completed.
