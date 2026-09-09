## 05 — Text Processing


**What It Is**

Text processing means searching through, filtering, transforming, and extracting information from text files entirely from the command line.

In Linux, almost everything is stored as text, log files, configuration files, user data. Being able to process text efficiently is one of the most powerful skills in the entire Linux ecosystem.


---
**Why It Matters in Security**

- Security logs contain millions of lines and manual reading is impossible
- grep through auth logs to find failed login attempts in seconds
- sed to automatically redact sensitive information from files
- awk to extract specific fields from structured log data
- These tools are used daily by SOC analysts, incident responders, and DevSecOps engineers


---
**The Analogy**

Imagine you have a warehouse of 10,000 filing boxes and you need to find every document that mentions a specific person's name, extract just their account numbers, and count how many times they appear.

Doing that manually would take weeks. These text processing tools do it in milliseconds. That is the power you are learning.


---
## Commands Learned

### grep — Search for Patterns
grep searches for lines that match a pattern.
```bash
grep "error" logfile.txt                    # find lines containing "error"
grep -i "error" logfile.txt                 # case insensitive search
grep -r "password" /etc/                    # search recursively through all files in /etc
grep -n "failed" auth.log                   # show line numbers of matches
grep -v "success" auth.log                  # show lines that do NOT match (invert)
grep -c "error" logfile.txt                 # count how many lines match
grep -l "password" /etc/*                   # list only filenames that contain matches
grep "Failed password" /var/log/auth.log    # real security use case
```
**Flags summary:**
- `-i` = case insensitive
- `-r` = recursive through directories
- `-n` = show line numbers
- `-v` = invert (exclude matches)
- `-c` = count matches
- `-l` = list filenames only
- `-A 2` = show 2 lines after each match
- `-B 2` = show 2 lines before each match


### pipe | — Chain Commands Together
The pipe sends the output of one command as input to the next.
```bash
ps aux | grep nginx             # list all processes, then filter for nginx
cat auth.log | grep "Failed"    # read file, then filter for failures
```


### cut — Extract Specific Columns
```bash
cut -d: -f1 /etc/passwd         # extract first field using : as delimiter
cut -d, -f2 data.csv            # extract second column from CSV
cut -c1-10 file.txt             # extract characters 1 through 10 from each line
```
**Flags:**
- `-d` = delimiter (what separates columns)
- `-f` = field (which column number)
- `-c` = character position


### sort — Sort Lines
```bash
sort file.txt                   # alphabetical sort
sort -r file.txt                # reverse sort
sort -n numbers.txt             # numeric sort
sort -u file.txt                # sort and remove duplicates
sort -k2 file.txt               # sort by second column
```


### uniq — Remove Duplicate Lines
```bash
uniq file.txt                         # remove consecutive duplicates
sort file.txt | uniq                  # sort first then remove all duplicates
uniq -c file.txt                      # count occurrences of each line
sort auth.log | uniq -c | sort -rn    # count and rank most frequent lines
```


### wc — Count Things
```bash
wc file.txt                         # lines, words, characters
wc -l file.txt                      # count lines only
grep "Failed" auth.log | wc -l      # count failed login attempts
```


### sed — Stream Editor (Find and Replace)
sed modifies text as it flows through.
```bash
sed 's/old/new/' file.txt                 # replace first occurrence per line
sed 's/old/new/g' file.txt                # replace ALL occurrences
sed 's/password/REDACTED/g' log.txt       # redact sensitive info
sed -n '5,10p' file.txt                   # print only lines 5 to 10
sed '/pattern/d' file.txt                 # delete lines matching pattern
sed -i 's/old/new/g' file.txt             # edit the file directly (in-place)
```
⚠️ `-i` modifies the actual file. Always make a backup first.


### awk — Advanced Text Processing
awk processes text column by column. Extremely powerful.
```bash
awk '{print $1}' file.txt                         # print first column of every line
awk '{print $1, $3}' file.txt                     # print first and third columns
awk -F: '{print $1}' /etc/passwd                  # use : as delimiter, print first field
awk '{print NR, $0}' file.txt                     # print line numbers with content
awk '/pattern/ {print}' file.txt                  # print lines matching pattern
awk '{sum += $1} END {print sum}' numbers.txt     # sum a column of numbers
```
- `$0` = entire line
- `$1, $2, $3` = first, second, third column
- `NR` = current line number
- `-F` = field separator


---
## Practical Examples

**Scenario: Find all failed SSH login attempts and count by IP address**
```bash
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn
```
This pipeline: finds failures → extracts IP column → sorts → counts each IP → sorts by count

**Scenario: Extract all usernames from the system**
```bash
cut -d: -f1 /etc/passwd | sort
```

**Scenario: Find the 10 most common errors in a log file**
```bash
grep "ERROR" application.log | sort | uniq -c | sort -rn | head -10
```

**Scenario: Redact all passwords from a config file before sharing**
```bash
sed 's/password=.*/password=REDACTED/g' config.txt
```


---
## Mistakes and Gotchas

**Mistake 1 — Forgetting to sort before uniq**
`uniq` only removes CONSECUTIVE duplicate lines. If duplicates are scattered through the file, sort first or you will miss them.

**Mistake 2 — Using sed -i without a backup**
`sed -i` modifies the original file permanently. Use `sed -i.bak` to create a backup automatically: `sed -i.bak 's/old/new/g' file.txt`

**Mistake 3 — Forgetting that awk columns start at $1 not $0**
`$0` is the whole line. `$1` is the first column. This trips up almost everyone initially.

**Mistake 4 — Overcomplicating with one tool when two would be cleaner**
A pipe chain of grep, cut, sort, uniq is often cleaner than trying to do everything in awk. Use the right tool for each step.


---
## Quick Revision

- `grep` searches for patterns — add `-i` for case insensitive, `-v` to invert
- `|` pipes output of one command into the next — this is how you chain tools
- `cut` extracts columns, `sort` orders lines, `uniq` removes duplicates
- `sed` finds and replaces text — use `-i` with caution
- `awk` processes column-based data — `$1`, `$2` etc. refer to columns


---
* *Previous: [06 — Process Management](06%20-%20Process%Management.md)*
* *Next: [08 — Advanced Text Processing & Scheduling](08%20-%20Advanced%20Text%Processing%20%26%20Scheduling.md)*
