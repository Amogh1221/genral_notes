# 🐧 Linux Commands & Shell Scripting

> **Interview-Ready | ML Engineer Focused | Practical + Theory**
> *Commonly tested in AMD, NVIDIA, and product company ML engineer interviews*

---

## Table of Contents

1. [File System & Navigation](#1-file-system--navigation)
2. [File Operations](#2-file-operations)
3. [Text Processing](#3-text-processing)
4. [Process Management](#4-process-management)
5. [Permissions & Ownership](#5-permissions--ownership)
6. [Networking Commands](#6-networking-commands)
7. [System Monitoring](#7-system-monitoring)
8. [Shell Scripting](#8-shell-scripting)
9. [SSH & Remote Access](#9-ssh--remote-access)
10. [ML Engineer Specific Commands](#10-ml-engineer-specific-commands)
11. [Interview Q&A](#11-interview-qa)

---

## 1. File System & Navigation

### Directory Structure

```
/           → Root directory
/home/user  → User's home directory (~)
/etc        → Configuration files
/var        → Variable data (logs: /var/log)
/tmp        → Temporary files (cleared on reboot)
/usr        → User programs (/usr/bin, /usr/lib)
/bin        → Essential binaries (ls, cp, mv)
/proc       → Virtual filesystem for process info (/proc/cpuinfo, /proc/meminfo)
/dev        → Device files (/dev/null, /dev/sda)
/opt        → Optional/third-party software
/mnt        → Mount points for external drives
```

### Navigation Commands

```bash
pwd                 # print working directory
cd /path/to/dir     # change directory (absolute path)
cd ..               # go up one level
cd ~                # go to home directory
cd -                # go to previous directory

ls                  # list files
ls -l               # long format (permissions, size, date)
ls -la              # include hidden files (starting with .)
ls -lh              # human-readable sizes (KB, MB, GB)
ls -lt              # sort by modification time (newest first)
ls -lS              # sort by size (largest first)
ls *.py             # list all Python files (glob pattern)

tree                # display directory tree
tree -L 2           # limit depth to 2 levels
```

---

## 2. File Operations

```bash
# Create
touch file.txt              # create empty file (or update timestamp)
mkdir my_dir                # create directory
mkdir -p path/to/nested     # create parent directories if needed

# Copy
cp file.txt backup.txt      # copy file
cp -r src/ dest/            # copy directory recursively
cp -p file.txt backup.txt   # preserve permissions and timestamps

# Move / Rename
mv old.txt new.txt          # rename file
mv file.txt /path/to/dir/   # move file to directory
mv *.py python_files/       # move all .py files to directory

# Delete (CAREFUL — no recycle bin in Linux!)
rm file.txt                 # delete file
rm -r directory/            # delete directory recursively
rm -rf directory/           # force delete (no confirmation) — very dangerous!
rm *.log                    # delete all .log files

# Viewing file content
cat file.txt                # print entire file
cat -n file.txt             # with line numbers
head file.txt               # first 10 lines
head -n 20 file.txt         # first 20 lines
tail file.txt               # last 10 lines
tail -n 50 file.txt         # last 50 lines
tail -f app.log             # follow file in real-time (great for logs!)
less file.txt               # paginated view (q to quit, / to search)
more file.txt               # paginated view (space to advance)
wc -l file.txt              # count lines
wc -w file.txt              # count words

# Finding files
find . -name "*.py"              # find all .py files in current dir
find . -name "*.log" -size +10M  # find .log files larger than 10MB
find . -mtime -7                 # files modified in last 7 days
find . -type d -name "__pycache__"  # find directories named __pycache__
locate filename                  # fast find using index (needs updatedb)

# Links
ln -s /path/to/original link_name   # create symbolic link (like Windows shortcut)
```

---

## 3. Text Processing

> **Interview Q:** *How would you find all lines containing "error" in a log file and count them?*

### grep — Search Text

```bash
# Basic
grep "error" app.log              # find lines containing "error"
grep -i "error" app.log           # case-insensitive
grep -r "model.predict" src/      # recursive search in directory
grep -n "error" app.log           # show line numbers
grep -c "error" app.log           # count matching lines
grep -v "DEBUG" app.log           # invert — lines NOT matching
grep -l "import torch" *.py       # only show filenames
grep -A 2 "error" app.log         # 2 lines After the match
grep -B 2 "error" app.log         # 2 lines Before the match
grep -E "error|warning" app.log   # extended regex (OR)
grep "^Error" app.log             # lines STARTING with "Error"
grep "\.py$" file_list.txt        # lines ENDING with ".py"

# Count errors in ML training log
grep -c "CUDA out of memory" training.log

# Find all Python files importing torch
grep -rl "import torch" . --include="*.py"
```

### awk — Column-Based Processing

```bash
# awk processes text column by column
# $0 = whole line, $1 = first column, $2 = second column, etc.
# Default delimiter: whitespace

awk '{print $1}' file.txt          # print first column
awk '{print $1, $3}' file.txt      # print columns 1 and 3
awk -F',' '{print $2}' data.csv    # CSV: use comma as delimiter
awk 'NR==5' file.txt               # print line 5 (NR = line number)
awk 'NR>=5 && NR<=10' file.txt     # print lines 5-10
awk '/error/{print}' app.log       # print lines containing "error"
awk '{sum+=$2} END{print sum}' data.txt  # sum column 2
awk '{if($3 > 0.9) print $0}' results.csv  # filter rows where col3 > 0.9

# Real ML example: Parse training log to extract epoch and loss
# Log line: "Epoch 5/100 - loss: 0.3421 - val_loss: 0.4123"
awk '/Epoch/{print $2, $4, $6}' training.log
# Output: 5/100 0.3421 0.4123

# Count GPU memory per process from nvidia-smi output
nvidia-smi | awk '/MiB/{print $3, $9}'
```

### sed — Stream Editor (Find & Replace)

```bash
# Basic substitution: s/old/new/
sed 's/old_text/new_text/' file.txt        # replace first occurrence per line
sed 's/old_text/new_text/g' file.txt       # replace ALL occurrences
sed 's/old_text/new_text/gi' file.txt      # case-insensitive
sed -i 's/old/new/g' file.txt              # edit file IN-PLACE (modifies file)
sed -i.bak 's/old/new/g' file.txt          # edit in-place, save backup as file.txt.bak

# Delete lines
sed '/pattern/d' file.txt                  # delete lines matching pattern
sed '5d' file.txt                          # delete line 5
sed '5,10d' file.txt                       # delete lines 5-10

# Print specific lines
sed -n '10,20p' file.txt                   # print lines 10-20

# Real ML example: Update learning rate in config file
sed -i 's/learning_rate: 0.001/learning_rate: 0.0001/' config.yaml

# Remove blank lines
sed '/^$/d' file.txt
```

### sort & uniq

```bash
sort file.txt               # sort alphabetically
sort -n file.txt            # sort numerically
sort -r file.txt            # reverse sort
sort -k 2 file.txt          # sort by second column
sort -k 2 -n file.txt       # sort by second column numerically
sort -u file.txt            # sort and remove duplicates

uniq file.txt               # remove consecutive duplicates (use after sort)
sort file.txt | uniq        # remove all duplicates
sort file.txt | uniq -c     # count occurrences of each unique line
sort file.txt | uniq -d     # show only duplicated lines

# Find most frequent error types in log
grep "ERROR" app.log | awk '{print $4}' | sort | uniq -c | sort -rn | head -10
```

### cut, paste, join

```bash
cut -d',' -f1,3 data.csv    # extract columns 1 and 3 from CSV
cut -c1-10 file.txt         # extract characters 1-10 of each line
```

### Piping & Redirection

```bash
# Pipe: send output of one command to input of another
grep "error" app.log | wc -l               # count error lines
cat data.csv | sort | uniq | head -20      # chain multiple commands

# Redirection
command > output.txt        # redirect stdout to file (overwrite)
command >> output.txt       # redirect stdout to file (append)
command 2> error.log        # redirect stderr to file
command > out.txt 2>&1      # redirect both stdout AND stderr to file
command < input.txt         # feed file as input to command

# /dev/null: black hole — discard output
command > /dev/null 2>&1    # suppress all output (useful in scripts)

# tee: write to file AND stdout simultaneously
python train.py | tee training.log   # see output AND save it
```

---

## 4. Process Management

> **Interview Q:** *How would you find which process is using the most CPU? How do you kill a process?*

```bash
# View processes
ps aux                  # all processes with details
ps aux | grep python    # find python processes
ps -ef                  # alternative format

# Process info columns (ps aux):
# USER   PID  %CPU  %MEM  VSZ  RSS  TTY  STAT  START  TIME  COMMAND

# Dynamic process viewer
top                     # real-time process view (q to quit)
htop                    # better version of top (may need install)

# Find process by name/port
pgrep python            # get PID of processes named python
pgrep -l python         # with names
pidof python            # similar
lsof -i :8080           # which process is using port 8080?
ss -tulnp | grep 8080   # modern alternative to netstat

# Kill processes
kill <PID>              # send SIGTERM (graceful shutdown — process can clean up)
kill -9 <PID>           # send SIGKILL (force kill — no cleanup)
kill -15 <PID>          # same as kill — SIGTERM
killall python          # kill all processes named python
pkill -f "train.py"     # kill processes matching pattern in command line

# Run in background
python train.py &                   # run in background, attached to terminal
nohup python train.py &             # run in background, persists after logout
nohup python train.py > out.log &   # save output and run in background
jobs                                # list background jobs in current session
fg %1                               # bring job 1 to foreground
bg %1                               # send job 1 to background

# Screen / tmux for persistent sessions
screen                              # start screen session
screen -d -m python train.py        # run in detached screen
screen -r                           # reattach to screen session
# Or use tmux (more powerful)
```

### Priority & Resource Limits

```bash
nice -n 10 python train.py          # run with lower priority (0=normal, 19=lowest)
nice -n -10 python train.py         # higher priority (needs sudo for negative)
renice 15 -p <PID>                  # change priority of running process

# Check resource usage of a process
cat /proc/<PID>/status              # detailed process info
cat /proc/<PID>/limits              # resource limits
```

---

## 5. Permissions & Ownership

> **Interview Q:** *What does `chmod 755` mean?*

### Permission System

```
File permissions: rwxrwxrwx
                  │││││││││
                  ││││││└┴┘ → others (everyone else): r-x (read + execute)
                  │││└┴┘   → group: r-x (read + execute)
                  └┴┘       → owner: rwx (read + write + execute)

r = read    = 4
w = write   = 2
x = execute = 1

755 = 7(owner: rwx) 5(group: r-x) 5(others: r-x)
644 = 6(owner: rw-) 4(group: r--) 4(others: r--)
600 = 6(owner: rw-) 0(group: ---) 0(others: ---)
777 = everyone has full permissions (avoid for sensitive files!)
```

```bash
# View permissions
ls -l file.txt
# -rw-r--r-- 1 amogh amogh 1234 Jan 1 10:00 file.txt
#  │││││││││     │     │
#  └──────┘└┘   owner  group

# Change permissions
chmod 755 script.sh         # numeric mode
chmod +x script.sh          # add execute permission to all
chmod u+x script.sh         # add execute for owner only
chmod go-w file.txt         # remove write for group and others
chmod -R 755 directory/     # recursive

# Change ownership
chown user:group file.txt   # change owner and group
chown -R amogh:amogh /data/ # recursive change
sudo chown root:root secret  # change to root

# Special permissions
chmod +s binary             # SUID: run as owner (e.g., passwd command)
chmod 1777 /tmp             # Sticky bit: only owner can delete in /tmp
```

---

## 6. Networking Commands

```bash
# Connectivity
ping google.com             # check if host is reachable
ping -c 4 google.com        # send exactly 4 pings
traceroute google.com       # trace route to host
nslookup google.com         # DNS lookup
dig google.com              # detailed DNS lookup
curl https://api.example.com/health       # make HTTP request
curl -X POST -H "Content-Type: application/json" \
     -d '{"text":"hello"}' \
     https://api.example.com/predict      # POST request

wget https://example.com/file.zip         # download file
wget -O myfile.zip https://example.com/f.zip  # download with specific name

# Port and network info
netstat -tulnp              # listening ports (old)
ss -tulnp                   # listening ports (modern, preferred)
lsof -i :8080               # what's on port 8080?
lsof -i TCP                 # all TCP connections

# Network interfaces
ifconfig                    # view network interfaces (old)
ip addr show                # view network interfaces (modern)
ip route show               # routing table

# Transfer files
scp file.txt user@server:/path/     # copy file to remote server
scp -r dir/ user@server:/path/      # copy directory to remote
scp user@server:/path/file.txt .    # download from remote
rsync -avz src/ user@server:/dest/  # smart sync (only changed files)
```

---

## 7. System Monitoring

```bash
# CPU, Memory, Disk
top                         # real-time CPU and memory
htop                        # better top
free -h                     # memory usage (human readable)
vmstat 1                    # CPU/memory stats every 1 second
df -h                       # disk usage by filesystem
df -h /home                 # specific filesystem
du -sh directory/           # directory size
du -sh *                    # size of each item in current dir
du -sh * | sort -h          # sorted by size

# System info
uname -a                    # kernel info
cat /proc/cpuinfo           # CPU details
cat /proc/meminfo           # memory details
nproc                       # number of CPU cores
lscpu                       # detailed CPU info
lsmem                       # memory info

# GPU (ML crucial!)
nvidia-smi                  # GPU status (usage, memory, temp)
nvidia-smi -l 1             # refresh every 1 second
nvidia-smi --query-gpu=utilization.gpu,memory.used,memory.total \
           --format=csv     # query specific metrics
watch -n 1 nvidia-smi       # watch nvidia-smi every second

# Disk I/O
iostat 1                    # I/O stats every 1 second
iotop                       # top-like view for disk I/O

# Log files
journalctl -f               # follow system logs
journalctl -u myservice     # logs for specific service
tail -f /var/log/syslog     # system log
tail -f /var/log/app.log    # application log
```

---

## 8. Shell Scripting

### Bash Script Basics

```bash
#!/bin/bash
# ^ shebang: tells OS which interpreter to use

# Variables (no spaces around =)
NAME="Amogh"
NUM=42
echo "Hello $NAME, you are $NUM years old"
echo "Hello ${NAME}!"        # curly braces for clarity

# Command substitution
CURRENT_DATE=$(date +%Y-%m-%d)
NUM_FILES=$(ls *.py | wc -l)
echo "Today: $CURRENT_DATE"

# Arithmetic
RESULT=$((5 + 3))
RESULT=$((NUM * 2))

# Reading input
read -p "Enter model name: " MODEL_NAME

# Special variables
$0   # script name
$1   # first argument
$@   # all arguments
$#   # number of arguments
$?   # exit code of last command (0 = success, non-zero = error)
$$   # current process ID
```

### Conditionals

```bash
# If statement
if [ "$1" == "train" ]; then
    echo "Starting training..."
    python train.py
elif [ "$1" == "serve" ]; then
    echo "Starting server..."
    python serve.py
else
    echo "Unknown command: $1"
    exit 1
fi

# File tests
if [ -f "model.pkl" ]; then echo "Model exists"; fi
if [ -d "data/" ]; then echo "Data dir exists"; fi
if [ -z "$VARIABLE" ]; then echo "Variable is empty"; fi
if [ -n "$VARIABLE" ]; then echo "Variable is not empty"; fi

# Numeric comparisons
if [ $NUM -gt 100 ]; then echo "Greater than 100"; fi
# -eq (equal), -ne (not equal), -lt (less than), -le (less or equal)
# -gt (greater than), -ge (greater or equal)

# String comparisons
if [ "$STR" == "hello" ]; then echo "Match"; fi
if [ "$STR" != "hello" ]; then echo "No match"; fi
```

### Loops

```bash
# For loop
for i in 1 2 3 4 5; do
    echo "Step $i"
done

for file in *.py; do
    echo "Processing $file"
    python lint.py "$file"
done

for i in $(seq 1 10); do
    echo $i
done

# C-style for loop
for ((i=0; i<10; i++)); do
    echo $i
done

# While loop
while [ $COUNT -lt 10 ]; do
    echo $COUNT
    COUNT=$((COUNT + 1))
done

# Read file line by line
while IFS= read -r line; do
    echo "$line"
done < input.txt
```

### Functions

```bash
# Define function
train_model() {
    local model_type=$1    # local: scoped to function
    local epochs=$2
    echo "Training $model_type for $epochs epochs"
    python train.py --model "$model_type" --epochs "$epochs"
    return $?              # return exit code
}

# Call function
train_model "bert" 10
if [ $? -eq 0 ]; then echo "Training succeeded"; fi
```

### Practical ML Script Example

```bash
#!/bin/bash
# train_and_evaluate.sh — Run training pipeline

set -e              # exit immediately if any command fails
set -u              # error on undefined variables
set -o pipefail     # pipe fails if any command in pipe fails

# Configuration
MODEL="bert-base"
EPOCHS=10
BATCH_SIZE=32
OUTPUT_DIR="./outputs/$(date +%Y%m%d_%H%M%S)"

mkdir -p "$OUTPUT_DIR"
echo "Output directory: $OUTPUT_DIR"

# Check GPU availability
if nvidia-smi &> /dev/null; then
    DEVICE="cuda"
    echo "GPU detected: $(nvidia-smi --query-gpu=name --format=csv,noheader)"
else
    DEVICE="cpu"
    echo "No GPU detected, using CPU"
fi

# Training
echo "Starting training..."
python train.py \
    --model "$MODEL" \
    --epochs "$EPOCHS" \
    --batch-size "$BATCH_SIZE" \
    --device "$DEVICE" \
    --output-dir "$OUTPUT_DIR" \
    2>&1 | tee "$OUTPUT_DIR/training.log"

echo "Training complete. Evaluating..."
python evaluate.py --model-dir "$OUTPUT_DIR"

echo "Done! Results in $OUTPUT_DIR"
```

### Cron Jobs — Scheduled Tasks

```bash
# Edit crontab
crontab -e      # edit current user's cron jobs
crontab -l      # list current cron jobs

# Format: minute hour day month weekday command
# *       *    *   *     *        = every minute of every hour of every day

# Examples:
# Run every day at 2 AM
0 2 * * * /home/amogh/scripts/backup_models.sh

# Run every Monday at 9 AM
0 9 * * 1 python /path/to/weekly_report.py

# Run every 6 hours
0 */6 * * * python /path/to/retrain.py

# Run on the 1st of every month at midnight
0 0 1 * * python /path/to/monthly_cleanup.py

# Log cron output
0 2 * * * /path/to/script.sh >> /var/log/myjob.log 2>&1
```

---

## 9. SSH & Remote Access

```bash
# Connect to remote server
ssh user@hostname               # basic SSH
ssh user@192.168.1.100         # by IP
ssh -p 2222 user@hostname      # custom port
ssh -i ~/.ssh/mykey.pem user@hostname  # specific key file

# Key-based authentication (safer than passwords)
ssh-keygen -t rsa -b 4096 -C "your@email.com"  # generate key pair
ssh-copy-id user@hostname       # copy public key to remote server
# Now you can SSH without password

# SSH tunneling (port forwarding)
ssh -L 8080:localhost:8080 user@server  # forward local 8080 to remote 8080
# Useful for: accessing Jupyter notebook on remote GPU server
ssh -L 8888:localhost:8888 user@gpu-server
# Then open localhost:8888 in your browser

# SSH config file (~/.ssh/config)
Host gpu-server
    HostName 192.168.1.100
    User amogh
    IdentityFile ~/.ssh/ml_key.pem
    Port 22

# Then just: ssh gpu-server

# SCP and rsync
scp model.pkl user@server:~/models/                     # upload
scp user@server:~/models/model.pkl ./                   # download
rsync -avz --progress ./data/ user@server:~/data/       # sync with progress
rsync -avz --exclude='.git' ./project/ user@server:~/  # exclude .git
```

---

## 10. ML Engineer Specific Commands

```bash
# GPU monitoring
nvidia-smi                          # GPU status snapshot
nvidia-smi -l 1                     # refresh every second
watch -n 0.5 nvidia-smi            # every 0.5 seconds
nvidia-smi --query-gpu=gpu_name,memory.used,memory.total,utilization.gpu \
           --format=csv,noheader    # custom metrics

# Check CUDA version
nvcc --version
cat /usr/local/cuda/version.txt

# Python environment
which python                        # which python is being used?
python --version
pip list | grep torch               # check installed PyTorch
pip list --outdated                 # which packages need updating

# Monitor Python process
ps aux | grep python                # find python processes
top -p $(pgrep -d',' python)       # monitor specific python PIDs

# Disk usage for ML
du -sh ~/data/                      # size of data directory
du -sh ~/models/                    # size of models directory
df -h                               # overall disk usage

# Kill training job gracefully
kill -SIGINT $(pgrep -f "train.py") # sends Ctrl+C to training script

# Count lines in dataset files
wc -l train.csv
wc -l *.jsonl

# Quick data inspection
head -5 train.csv                   # first 5 rows
cut -d',' -f1,2 train.csv | head   # first two columns
awk -F',' '{print NF}' train.csv | sort | uniq -c  # check column counts (CSV consistency)

# Find large files eating disk
find . -size +1G -type f 2>/dev/null | sort
du -sh * | sort -rh | head -20

# Environment variables
export CUDA_VISIBLE_DEVICES=0       # use only GPU 0
export CUDA_VISIBLE_DEVICES=0,1     # use GPUs 0 and 1
export CUDA_VISIBLE_DEVICES=""      # use CPU only
printenv                            # list all env vars
echo $PATH

# Background training with output
nohup python train.py --epochs 100 > training.log 2>&1 &
echo "Training started with PID $!"
tail -f training.log                # follow the log
```

---

## 11. Interview Q&A

**Q: What does `chmod 755` mean?**
> Permissions are split into three groups: owner, group, others. Each digit is the sum of: 4 (read) + 2 (write) + 1 (execute). `755` = owner has `7` (4+2+1 = rwx), group has `5` (4+1 = r-x), others have `5` (r-x). The file owner can read, write, execute. Everyone else can read and execute but not write. Common for scripts and executables.

**Q: What is the difference between `>` and `>>` in Linux?**
> `>` redirects output to a file and **overwrites** it. `>>` redirects output and **appends** to the end of the file. Example: `echo "line" > file.txt` (replaces file content), `echo "line" >> file.txt` (adds to end). For logging, always use `>>` so you don't lose previous logs.

**Q: How would you find all processes using more than 10% CPU?**
> `ps aux | awk '$3 > 10 {print $0}'` — uses ps to list all processes, awk to filter where the %CPU column (column 3) is greater than 10. Alternatively use `top` interactively and sort by CPU (`P` key).

**Q: What is a soft link vs hard link?**
> A **hard link** is another name for the same file (same inode). Both names point to the same data on disk. Deleting one doesn't delete the data as long as another hard link exists. Cannot cross filesystems. **Soft link (symbolic link)** is a pointer to another file's path. If the original is deleted, the soft link breaks. Can cross filesystems. `ln -s original link_name` creates a soft link.

**Q: What does `2>&1` mean?**
> `2` is the file descriptor for stderr. `1` is stdout. `2>&1` redirects stderr to wherever stdout is currently pointing. `command > file.txt 2>&1` sends both stdout and stderr to file.txt. The order matters: `command 2>&1 > file.txt` is different (stderr goes to original stdout, then stdout goes to file).

**Q: How would you find which process is listening on port 8080?**
> `lsof -i :8080` or `ss -tulnp | grep 8080` or `netstat -tulnp | grep 8080`. These show the PID and process name using that port. Very useful when you get "Address already in use" when starting your ML server.

**Q: What is the difference between kill and kill -9?**
> `kill <PID>` sends SIGTERM (signal 15) — a polite request to terminate. The process can catch this signal, clean up resources (flush buffers, save state), and exit gracefully. `kill -9 <PID>` sends SIGKILL — cannot be caught or ignored. The OS immediately terminates the process. No cleanup possible. Use SIGTERM first; use SIGKILL only if the process ignores SIGTERM. For ML: SIGTERM allows training to save checkpoints; SIGKILL does not.

**Q: How do you monitor GPU usage in real time?**
> `watch -n 1 nvidia-smi` runs `nvidia-smi` every second. For more detail: `nvidia-smi --query-gpu=utilization.gpu,memory.used,memory.total,temperature.gpu --format=csv -l 1` shows GPU utilisation, memory usage, and temperature refreshed every second. `nvitop` is a better interactive GPU monitor (pip install nvitop).

---

*Sources: Turing Linux Interview Questions 2025, InterviewBit Linux Questions, GeeksforGeeks Linux Commands, AMD AI Engineer Interview Guide, UnderCode Testing Linux Scenario Questions.*
