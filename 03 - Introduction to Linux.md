## 01 - Introduction to Linux


**What It Is**

Just like Windows, iOS, & Mac OS, **Linux** is a free, open-source operating system that runs on almost everything (from Android phones to the world’s most powerful servers).
An **Operating System** is a software that sits between you and the hardware. When you click on something on your screen, the **OS** (Operating System) is what translates that click into something that the processor and the memory can understand and act on.
You probably have used Android, Windows, iOS, macOS, etc. your whole life. **Linux** works the same way.
To put is simply, the operating system manages the communication between your software and your hardware and Linux is one of most used Operating systems in the world.

**The Three most important thing to know about Linux:**
1.	It is open source. Anyone can read, modify and distribute the code.
2.	It is case sensitive. File.txt and file.txt are completely different files.
3.	Almost everything in a file.

---
**Why It Matters in Security**

* Almost every server, Infrastructure and containers runs on Linux.
* Most of the tools and platforms are Linux-native.
* The terminal is faster, lighter, scriptable and works with low-bandwidth connections.
* As a DevSecOps engineer you will spend the majority of your time in a Linux terminal.

---
**The Analogy**

Think of Linux like the commercial kitchen of a restaurant.
Windows and macOS are the dining room; comfortable, curated, you order from a fixed menu. Linux is the kitchen; no decorations, raw ingredients, open flames, and where the actual creation happens. Most diners never go there. Every chef does.

---
**The Linux File System – How it is organized**

Unlike Windows which uses c:\ drives, Linux has one single tree starting from / called root.
```text
        /                   ← Root — the top of everything
        ├── /home           ← Personal files for each user (like Windows Users folder)
        ├── /root           ← Home folder specifically for the root (admin) user
        ├── /etc            ← Configuration files — system settings live here
        ├── /var            ← Variable data — logs, databases, emails
        ├── /tmp            ← Temporary files — cleared on reboot
        ├── /bin            ← Essential commands available to all users
        ├── /sbin           ← System commands for administrators only
        ├── /usr            ← Installed software and libraries
        ├── /dev            ← Device files — hardware represented as files
        ├── /proc           ← Running processes — a virtual filesystem
        └── /opt            ← Optional third-party software
```
---
## Commands Learned

### pwd – Print Working Directory
Shows you exactly where you are currently in the file system.
```bash
pwd
# output: /home/tuhin
```
For eg. -> /home/tuhin : under root, inside home, and inside tuhin (user).

Think of it asking “Where am I standing right now?”

### ls – List Directory Contents
Shows what is inside the current folder ( file/ directory )
```bash
ls                 # basic list
ls -l              # long format — shows permissions, owner, size, date
ls -a              # shows hidden files (files starting with .)
ls -la             # combines both — long format AND hidden files
ls -lh             # human readable file sizes (KB, MB instead of bytes)
ls /etc            # list contents of a specific folder without going there
```
### flag Beakdown:
* -l = long listing format 
* -a = all files including hidden 
* -h = human readable sizes
* -t = sort by modification time (newest first)
You can also combine more than 2 or all the flags.

For eg. ->
```bash
ls -alht /etc             # shows list of all files (including .hidden file) in long listing and human readable sizes which is sorted by modification time (newest first)
```

### cd – Change Directory
Moves you from one folder to another.
```bash
cd /home/tuhin                 # go to a specific path (absolute path)
cd documents                   # go into documents from/to current location (relative path)
cd ..                          # go to one level up
cd ../..                       # go to two level up
cd ~                           # go to your home directory from anywhere
cd -                           # go back to previous directory
cd /                           # go to root
```
### Absolute vs Relative paths:
* Absolute path means the entire path from root to target; It usually starts from / and works anywhere.
* Relative path does not starts with / and only works from where you currently are.

### Man – Manual 
Opens the manual pages for any command. The most important tool.
```bash
man ls 		        # opens the full manual for ls
man chmod	        # manual for chmod
```
It has all the flags and detailed files about a command.
Press ‘q’ to quit. 

### clear
Clears the terminal screen. Shortcut: ctrl + L

### history
Shows you your previously run commands.
```bash
history                  # shows all previous commands with numbers
history	20               # shows last 20 commands
!42                      # re-runs command number 42 from history
!!                       # re-run your last command.
```
You can also use arrow keys to traverse the history. 
Up arrow -> previous command.
Down arrow -> next command.

## Practical Example 
### Scenario: You log into a Linux server for the first time. What do you do ?
```bash 
pwd                   # find out where you are
ls -al                # see everything in current directory including .hidden files
cd /etc               # navigate to configuration files
ls -l                 # see what config files exist
cd ~                  # come back home
```
### Scenario: You want to find a specific folder called ‘logs’
```bash
Ls -al /var	    # checks if logs folder is in /var
```

## Mistakes and Gotchas
* Mistake 1 - This is the most common mistake which I personally made – Forgetting Linux is case sensitive ‘cd Documents’ and ‘cd documents’ are different commands if the folder is named ‘document’. 
* Mistake 2 - Confusing absolute path and relative paths. If you type ‘cd home/tuhin’ without leading ‘/’ and you are not already at root, it will fail. Absolute path should always begin with ‘/’.
* Mistake 3 - Not using man pages; Every beginner googles commands. Every experienced engineer checks man first. Build the habit now.

## Quick Revision
* Linux file system starts at / called root and everything branches from here.
* pwd tells you where you are.
* ls shows what is there.
* cd moves you.
* Linux is case-sensitive; which means File.txt and file.txt are different files.
* Hidden files start with a dot (.).
* use ls -a to see them.
* man <command> opens the manual for any command. It also has all the flags related to the command.

---
**Next:** [04 — File Management](04%20-%20File%20Management.md)


