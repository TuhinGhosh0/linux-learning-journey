## 03 — User Management & Permissions


**What It Is**

Linux is a multi-user operating system. Multiple people can use the same system simultaneously, and each person has their own account, files, and level of access.

Permissions control who can read, write, or execute any file or folder. This is one of the most important security concepts in all of Linux.


---
**Why It Matters in Security**

- Most privilege escalation attacks exploit misconfigured permissions
- **The principle of least privilege** —> giving users only what they need. It is a core security concept
- Attackers frequently try to escalate from regular user to root; Understanding this helps you defend against it


---
**The Analogy**

Think of a large office building.

- The **building** is your Linux system
- Each **employee** is a user with a keycard
- **Rooms** are files and directories
- **Permissions** are which doors each keycard can open
- The **security manager** is root. They can access every room

Your job as a security engineer is to make sure no keycard opens more doors than it should.


---
**Understanding Users and Groups**


**Root user** — the superuser. Has unrestricted access to everything on the system. Like a system administrator with a master key.

**Regular users** — have limited access. Can only affect their own files unless given specific permissions.

**Groups** — collections of users. You can assign permissions to a group instead of individually to each user.

Every file in Linux has three associated identities:
- **Owner** — the user who created it
- **Group** — a group of users
- **Others** — everyone else on the system


---
**Understanding Permission Notation**

When you run `ls -l` you see something like this:

```
-rwxr-xr-- 1 tuhin developers 4096 Sep 07 10:00 script.sh
```

Breaking this down:

```
- rwx r-x r--
│ │   │   │
│ │   │   └── Others permissions (everyone else)
│ │   └────── Group permissions (developers)
│ └────────── Owner permissions (tuhin)
└──────────── File type (- = file, d = directory, l = symlink)
```

**Permission characters:**
- `r` = read (value: 4) — can view the file
- `w` = write (value: 2) — can modify the file
- `x` = execute (value: 1) — can run the file as a program
- `-` = permission not granted


---
**Numeric Permission System**

Each permission has a number value. Add them together:

| Permission | Value |
|-----------|-------|
| read (r) | 4 |
| write (w) | 2 |
| execute (x) | 1 |
| none (-) | 0 |

The numbers for read, write and execute are from binary
```text
    r   w   e
    1   0   0  ->  4 (read)
    0   1   0  ->  2 (write)
    0   0   1  ->  1 (execute)
```

**Common combinations:**
- `7` = rwx = 4+2+1 = full access
- `6` = rw- = 4+2 = read and write
- `5` = r-x = 4+1 = read and execute
- `4` = r-- = 4 = read only
- `0` = --- = no access

**Example:** `chmod 755 script.sh` means:
- Owner: 7 = rwx (full access)
- Group: 5 = r-x (read and execute)
- Others: 5 = r-x (read and execute)


---
## Commands Learned

### whoami — Who Am I
```bash
whoami              # shows your current username
```

### id — User Identity Information
```bash
id                  # shows your user ID, group ID, and all groups you belong to
id username         # shows information for a specific user
```

### useradd — Create a New User
```bash
sudo useradd loki                       # creates user loki
sudo useradd -m loki                    # creates user with home directory
sudo useradd -m -s /bin/bash loki       # with home directory and bash shell
sudo useradd -m -G developers loki      # adds to group on creation
```

### passwd — Set Password
```bash
sudo passwd loki        # set password for user loki
passwd                  # change your own password
```

### usermod — Modify a User
```bash
sudo usermod -aG developers loki    # add loki to developers group
sudo usermod -s /bin/bash loki      # change loki's shell
sudo usermod -l newname loki        # rename user
```
⚠️ Always use `-aG` not `-G` when adding to groups. `-G` alone REPLACES all existing groups. `-aG` APPENDS to existing groups.

### userdel — Delete a User
```bash
sudo userdel loki           # delete user but keep home directory
sudo userdel -r loki        # delete user AND their home directory
```

### groupadd — Create a Group
```bash
sudo groupadd developers    # creates a new group called developers
```

### groups — See Group Membership
```bash
groups              # see which groups you belong to
groups loki         # see which groups loki belongs to
```

### chmod — Change File Permissions
```bash
chmod 755 script.sh             # numeric method
chmod u+x script.sh             # add execute for owner (u=user/owner)
chmod g-w file.txt              # remove write from group
chmod o-r file.txt              # remove read from others
chmod a+r file.txt              # add read for all (a=all)
chmod -R 755 projects/          # apply recursively to entire folder
```

### chown — Change File Owner
```bash
sudo chown loki file.txt            # change owner to loki
sudo chown loki:developers file.txt # change owner to loki, group to developers
sudo chown -R loki projects/        # change ownership of entire folder
```

### sudo — Run as Superuser
The name sudo stands for "Superuser Do,".
```bash
sudo command            # run a single command as root
sudo -l                 # list what commands you are allowed to run as sudo
sudo su                 # switch to root user entirely (use carefully)
```

### cat /etc/passwd — View All Users
```bash
cat /etc/passwd         # shows all user accounts on the system
```
Format: `username:x:UID:GID:comment:home:shell`

### cat /etc/group — View All Groups
```bash
cat /etc/group          # shows all groups and their members
```


---
## Practical Examples

**Scenario: Setting up a new developer on your team**
```bash
sudo useradd -m -s /bin/bash -G developers alice
sudo passwd alice
groups alice                    # verify group membership
```

**Scenario: Making a script executable**
```bash
chmod +x deploy.sh              # add execute permission for everyone
ls -l deploy.sh                 # verify the permission change
```

**Scenario: Security audit — finding world-writable files**
```bash
find / -perm -002 -type f       # finds files anyone can write to; it is a security risk
```


---
## Mistakes and Gotchas

**Mistake 1 — Using usermod -G instead of -aG**
`usermod -G developers loki` removes loki from ALL other groups first, then adds to developers. This is almost never what you want hopefully. Always use `-aG`.

**Mistake 2 — chmod 777 on everything**
Setting 777 means anyone on the system can read, write, and execute the file. This is a severe security misconfiguration. Never ever do this in production.

**Mistake 3 — Working as root all the time**
If you are always root and make a mistake — like `rm -rf` in the wrong directory; there is nothing to stop you. Use a regular account with sudo for specific tasks.

**Mistake 4 — Forgetting sudo**
Many system commands require elevated privileges. If a command says Permission denied — try with sudo.


---
## Quick Revision

- Every file has an owner, a group, and permissions for others
- Permissions are read(4), write(2), execute(1). Add them for numeric notation
- `chmod` changes permissions
- `chown` changes ownership
- Always use `usermod -aG` not `-G` when adding users to groups
- Never use chmod 777 in production. It is a security disaster


---
*Previous: [04 — File Management](04%20-%20File%20Management.md)*
*Next: [06 — Process Management](06%20-%20Process%20Management.md)*
