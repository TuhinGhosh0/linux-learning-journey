## 04 — Process Management


**What It Is**

A process is any program that is currently running on a system. Every time someone opens an application, runs a command, or starts a service; a process is created.

Linux gives you complete visibility and control over every process running on your system.


---
**Why It Matters in Security**

- Malware runs as a process. Knowing what should and should not be running is critical
- Incident response often begins with: what processes are running right now that should not be?
- Resource exhaustion attacks (like fork bombs) target process management
- Monitoring processes is a core function of any SOC analyst or DevSecOps engineer


---
**The Analogy**

Think of your Linux system as a kitchen in a busy restaurant.

Every dish being cooked is a process. The head chef (root) can start or stop any dish at any time. Regular chefs (users) can only manage their own station. The order tickets are the commands. And if one dish is burning and consuming all the gas — you need to kill that process before it ruins everything else.

---

## Key Concepts

**PID — Process ID**
Every process gets a unique number when it starts. This is how Linux identifies and manages it.

**PPID — Parent Process ID**
Every process is started by another process. The PPID is the parent's PID.

**Foreground vs Background**
- Foreground process: occupies your terminal, you wait for it to finish
- Background process: runs independently, your terminal stays free

**Signals**
Signals are messages you send to a process to tell it what to do — pause, stop, terminate, etc.

---

## Commands Learned

### ps — Process Status
```bash
ps                      # shows processes in your current terminal session
ps aux                  # shows ALL processes from ALL users in detail
ps aux | grep nginx     # find a specific process by name
```
**Column meanings in ps aux:**
- `USER` = who owns the process
- `PID` = process ID
- `%CPU` = CPU usage percentage
- `%MEM` = memory usage percentage
- `COMMAND` = what command started the process

### top — Live Process Monitor
```bash
top                     # opens live updating process monitor
```
Controls inside top:
- `q` = quit
- `k` = kill a process (enter PID)
- `M` = sort by memory usage
- `P` = sort by CPU usage
- `u` = filter by username

### htop — Better Live Monitor
```bash
htop                    # improved version of top with colours and mouse support
```
May need to install: `sudo apt install htop`

### kill — Terminate a Process
```bash
kill PID                # send default termination signal to process
kill -9 PID             # force kill — immediate termination, no cleanup
kill -15 PID            # graceful termination — process can clean up first
```

### killall — Kill by Name
```bash
killall firefox         # kills all processes named firefox
killall -9 nginx        # force kills all nginx processes
```

### pgrep — Find Process by Name
```bash
pgrep nginx             # returns the PID of nginx
pgrep -u tuhin          # all processes owned by user tuhin
```

### pkill — Kill by Name
```bash
pkill nginx             # kill process by name without needing the PID
```

### jobs — See Background Jobs
```bash
jobs                    # lists all jobs running in background in current session
```

### bg and fg — Background and Foreground
```bash
command &               # start a command directly in background using &
Ctrl + Z                # pause a foreground process
bg                      # resume the paused process in background
fg                      # bring background process back to foreground
fg %2                   # bring specific job number 2 to foreground
```

### nice and renice — Process Priority
Linux schedules processes based on priority. Nice value ranges from -20 (highest priority) to 19 (lowest).

```bash
nice -n 10 command      # start command with lower priority (nice to others)
renice 10 -p PID        # change priority of running process
```

### nohup — Keep Running After Logout
```bash
nohup command &         # runs command that survives terminal closure
```

### systemctl — Manage System Services
```bash
sudo systemctl start nginx      # start a service
sudo systemctl stop nginx       # stop a service
sudo systemctl restart nginx    # restart a service
sudo systemctl status nginx     # check if service is running
sudo systemctl enable nginx     # start service automatically on boot
sudo systemctl disable nginx    # stop service from starting on boot
```

---

## Practical Examples

**Scenario: Something is slowing your server down — find what is consuming resources**
```bash
top                             # open live monitor
# Press M to sort by memory, P to sort by CPU
ps aux --sort=-%cpu | head -10  # top 10 CPU-consuming processes
```

**Scenario: You suspect a malicious process is running**
```bash
ps aux                          # see all running processes
ps aux | grep -v "expected"     # look for unexpected processes
ls -l /proc/PID/exe             # see what binary a process is actually running
```

**Scenario: A service crashed and you need to restart it**
```bash
sudo systemctl status nginx     # check current status
sudo systemctl restart nginx    # restart it
sudo systemctl status nginx     # verify it is running again
```

---

## Mistakes and Gotchas

**Mistake 1 — Using kill -9 as the first option**
Kill -9 is a forced kill. The process cannot clean up after itself. Always try `kill -15` (graceful) first. Only use -9 if the process refuses to stop.

**Mistake 2 — Killing a process by name when multiple instances are running**
`killall nginx` kills ALL nginx processes including ones you might need. Be specific with PIDs in production.

**Mistake 3 — Not knowing what a process does before killing it**
Never kill a process you do not recognise without researching it first. Some system processes are critical and killing them can crash the system.

**Mistake 4 — Forgetting the & when running background processes**
If you forget `&` and run a long process, your terminal is stuck until it finishes. Use `Ctrl + Z` then `bg` to send it to background.

---

## Quick Revision

- Every running program is a process with a unique PID
- `ps aux` shows all processes, `top` shows them live
- `kill PID` terminates a process — use `-15` for graceful, `-9` for force
- `systemctl` manages system services — start, stop, restart, status
- Security focus: always know what processes should be running and investigate anything unexpected


---
*Previous: [05 — User Management & Permissions](05%20-%20User%20Management%20%26%20Permissions.md)*
*Next: [07 — Text Processing](07%20-%20Text%20Processing.md)*
