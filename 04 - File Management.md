## 02 — File Management


**What It Is**

File management in Linux basically means creating, reading, moving, copying, deleting, and organizing files and directories entirely through the terminal; No mouse, no drag and drop.

This feels slow at first. But, it becomes faster than any GUIs once it is muscle memory.

---
**Why It Matters in Security**

- Log files, config files, and malware all live in the file system.
- Incident responders navigate file systems under pressure; So speed matters a lot.
- Understanding where files live helps you spot what should not be there.
- Copying and moving evidence files without corrupting them is a core forensics skill.

---
**The Analogy**

Think of Linux **File management** like organizing a physical filing cabinet; but with superpowers. You can copy an entire drawer in one command, search every document in the cabinet for one specific word, or move thousands of files in a single line. The commands are just shortcuts for things you already understand.

---
## Commands Learned

### touch — Create an Empty File
```bash
touch file.txt              # creates an empty file called file.txt
touch file1.txt file2.txt   # creates multiple files at once
```
Also updates the timestamp of an existing file without changing its contents.


### mkdir — Make Directory
```bash
mkdir projects                      # creates a folder called projects
mkdir -p projects/linux/notes       # creates nested folders in one command
```
**Flag:** `-p` = create parent directories as needed (no error if they already exist)


### cp — Copy
```bash
cp file.txt backup.txt              # copy file.txt and name the copy backup.txt
cp file.txt /home/tuhin/Documents/  # copy file to a different location
cp -r projects/ projects_backup/    # copy an entire folder and its contents
cp -i file.txt backup.txt           # Safety check: Ask "Are you sure?" before replacing an existing file
```
**Flags:**
- `-r` = recursive — required when copying folders
- `-i` = interactive — prompts before overwriting
- `-v` = verbose — shows what is being copied


### mv — Move or Rename
```bash
mv file.txt /home/tuhin/Documents/  # move file to Documents
mv oldname.txt newname.txt          # rename a file
mv projects/ /opt/                  # move entire folder
```
`mv` does not need `-r` for folders unlike `cp`.


### rm — Remove
```bash
rm file.txt                 # delete a file
rm -r projects/             # delete a folder and everything inside it
rm -i file.txt              # ask before deleting
rm -f file.txt              # force delete without asking
rm -rf projects/            # force delete folder and contents — NO confirmation
```
⚠️ **WARNING:** `rm -rf` has no undo. There is no recycle bin in Linux. Once deleted, it is gone. Always double-check before running this command.

**I believe it would be best practice to use `-iv` for loose files (safety breaks way) and -`rf` for entire directory (bulldozer way)**

**Make sure to triple check before deleting anything; cause it is possible to delete absolutely anything and everything.** 

⚠️ **WARNING:** Never ever run `sudo rm -rf /` also known as **Nuclear Option**. Running this command will completely destroy your computer's operating system beyond repair in just a few seconds.

### cat — Read a File
```bash
cat file.txt                # display entire file contents
cat file1.txt file2.txt     # display multiple files in sequence
cat -n file.txt             # display with line numbers
```


### less — Read Large Files Page by Page
```bash
less largefile.txt          # open file for scrolling
```
Controls inside less:
- `Space` = next page
- `b` = previous page
- `/word` = search for word
- `q` = quit


### head and tail — Read Parts of a File
```bash
head file.txt               # show first 10 lines
head -n 20 file.txt         # show first 20 lines
tail file.txt               # show last 10 lines
tail -n 20 file.txt         # show last 20 lines
tail -f logfile.txt         # follow a file in real time as it updates
```
**Security use:** `tail -f /var/log/auth.log` watches login attempts in real time.


### find — Search for Files
```bash
find / -name "file.txt"             # search entire system for file.txt
find /home -name "*.txt"            # find all .txt files in /home
find / -name "*.sh" -type f         # find all shell script files
find / -mtime -1                    # find files modified in last 24 hours
find / -perm 777                    # find files with full permissions (security risk)
```
**Security use:** `find / -perm 777` is one of the first commands a security auditor runs.


### file — Identify File Type
```bash
file document.txt           # tells you what type of file it actually is
file suspicious_file        # attackers often rename malware — this reveals the truth
```


### wc — Word Count
```bash
wc file.txt                 # shows lines, words, characters
wc -l file.txt              # count lines only
wc -w file.txt              # count words only
```


---
## Practical Examples

**Scenario: Organising your learning notes**
```bash
mkdir -p ~/notes/linux/week1                                                # makes a nested folder
touch ~/notes/linux/week1/day1.txt                                          # creates a file named day1.txt
cp ~/notes/linux/week1/day1.md ~/notes/linux/week1/day1_backup.txt          # duplicate the file and renames it as day1_backup.txt in same directory
```

**Scenario: Checking a log file for suspicious activity**
```bash
tail -f /var/log/auth.log           # watch live login attempts
grep "Failed" /var/log/auth.log     # find all failed login attempts
```

**Scenario: Finding recently modified files after a suspected breach**
```bash
find / -mtime -1 -type f            # what files changed in the last 24 hours?
```


---
## Mistakes and Gotchas

**Mistake 1 — Running rm -rf without checking the path**
Double-check your path before running. `rm -rf /` would attempt to delete your entire system.

**Mistake 2 — Forgetting -r when copying folders with cp**
`cp projects/ backup/` will fail. You need `cp -r projects/ backup/`

**Mistake 3 — Using cat on large files**
`cat` dumps the entire file at once. Use `less` for large files so you can scroll.

**Mistake 4 — Not knowing file vs filename extension**
In Linux, file extensions are optional and sometimes misleading. Use `file` command to confirm what a file actually is.


---
## Quick Revision

- `touch` creates files
- `mkdir` creates folders
- `mkdir -p` creates nested folders
- `cp` copies
- `mv` moves and renames
- `rm` deletes — permanently
- Always use `-r` with `cp` and `rm` when working with folders
- `tail -f` watches a file update in real time — essential for log monitoring
- `find / -perm 777` finds dangerously permissive files — a key security check


---
- *Previous: [01 — Introduction to Linux](../01-introduction-to-linux/README.md)*
- *Next: [03 — User Management & Permissions](../03-user-management-permissions/README.md)*
