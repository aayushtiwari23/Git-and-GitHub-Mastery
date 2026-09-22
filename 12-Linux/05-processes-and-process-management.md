
```bash
pidof bash
```

to find PIDs associated with a program.

---

## 11. `top`

`top` provides a live view of running processes.

Run:

```bash
top
```

It commonly displays:

```text
CPU usage
Memory usage
Processes
PIDs
Users
Load information
```

Press:

```text
q
```

to exit.

---

## 12. `htop`

If installed:

```bash
htop
```

provides an interactive process viewer.

It is often easier to read than `top`.

If it isn't installed, use:

```bash
top
```

---

## 13. Process States

Processes can have different states.

Common state concepts include:

```text
Running
Sleeping
Stopped
Zombie
```

The exact state representation appears in tools such as:

```bash
ps
top
```

---

## 14. Running Process

A running process is actively executing or ready to execute on the CPU.

It may be shown with a state such as:

```text
R
```

---

## 15. Sleeping Process

A sleeping process is waiting for an event or resource.

It may be shown with:

```text
S
```

Sleeping does not necessarily mean the process is broken.

Most applications spend significant amounts of time waiting.

---

## 16. Stopped Process

A stopped process is temporarily suspended.

You can create this state from the terminal with:

```text
Ctrl + Z
```

when a foreground process is running.

---

## 17. Zombie Process

A zombie is a process that has finished execution but whose parent has not yet collected its exit status.

It may appear as:

```text
Z
```

A zombie is not actively executing.

---

## 18. Parent and Child Processes

Processes can create other processes.

For example:

```text
Parent Process
      ↓
Child Process
      ↓
Another Child Process
```

Linux maintains parent-child relationships.

---

## 19. PPID

PPID means:

```text
Parent Process ID
```

You can see it with:

```bash
ps -ef
```

or:

```bash
ps -o pid,ppid,cmd
```

Example:

```text
PID   PPID   CMD
1200  1100   bash
1300  1200   python app.py
```

Here:

```text
python → child
bash   → parent
```

---

## 20. Process Tree

Use:

```bash
pstree
```

if available.

Example:

```text
systemd
 ├─ sshd
 │   └─ bash
 │       └─ python
 └─ system services
```

This helps visualize parent-child relationships.

---

## 21. Foreground Process

A foreground process occupies the current terminal.

Example:

```bash
python app.py
```

If the application keeps running, the terminal is occupied by that process.

---

## 22. Background Process

A process can run in the background.

Example:

```bash
python app.py &
```

The:

```text
&
```

runs the command in the background.

You can continue using the terminal.

---

## 23. `jobs`

View jobs started from the current shell:

```bash
jobs
```

Example:

```text
[1]+ Running python app.py &
```

---

## 24. `Ctrl + Z`

Press:

```text
Ctrl + Z
```

to suspend the current foreground process.

Example:

```text
Running process
      ↓
Ctrl + Z
      ↓
Stopped
```

---

## 25. `bg`

After suspending a process, use:

```bash
bg
```

to continue it in the background.

Example:

```text
Ctrl + Z
   ↓
bg
```

---

## 26. `fg`

Bring a background job back to the foreground:

```bash
fg
```

If you have multiple jobs:

```bash
fg %1
```

The number comes from:

```bash
jobs
```

---

## 27. Killing a Process

The `kill` command sends a signal to a process.

Example:

```bash
kill 1234
```

where:

```text
1234
```

is the PID.

---

## 28. SIGTERM

The default signal sent by:

```bash
kill PID
```

is normally:

```text
SIGTERM
```

SIGTERM asks the process to terminate gracefully.

This gives the application an opportunity to:

```text
Clean up
Close files
Save state
Exit properly
```

---

## 29. SIGKILL

You can force termination with:

```bash
kill -9 PID
```

This sends:

```text
SIGKILL
```

It cannot be caught or handled by the target process.

Use it only when necessary.

Prefer:

```bash
kill PID
```

before:

```bash
kill -9 PID
```

---

## 30. Common Signals

Some important signals are:

```text
SIGTERM → graceful termination
SIGKILL → force termination
SIGSTOP → stop process
SIGCONT → continue process
SIGHUP  → hangup
SIGINT  → interrupt
```

---

## 31. `Ctrl + C`

When you press:

```text
Ctrl + C
```

in a terminal, it normally sends:

```text
SIGINT
```

to the foreground process.

This commonly interrupts the program.

---

## 32. `killall`

You may see:

```bash
killall process-name
```

This targets processes by name.

Example:

```bash
killall example
```

Use it carefully because it can affect multiple processes.

---

## 33. `pkill`

Another option is:

```bash
pkill process-name
```

For example:

```bash
pkill example
```

It can terminate matching processes.

Always verify what will be matched before using process-killing commands.

---

## 34. Process Priority

Linux processes have scheduling priority information.

One important value is:

```text
nice
```

A process can be started with a specific niceness value.

Example:

```bash
nice -n 10 command
```

---

## 35. Niceness

Niceness influences CPU scheduling priority.

In general:

```text
Lower nice value
    ↓
Higher scheduling priority

Higher nice value
    ↓
Lower scheduling priority
```

The exact scheduling behaviour depends on the Linux scheduler and system conditions.

---

## 36. `renice`

You can change the niceness of an existing process.

Example:

```bash
renice 10 -p 1234
```

where:

```text
1234
```

is the PID.

Changing priority may require elevated privileges depending on the requested change.

---

## 37. Process Resources

A process can consume:

```text
CPU
Memory
Disk I/O
Network resources
File descriptors
```

If a system becomes slow, process inspection can help identify the cause.

---

## 38. CPU Usage

Use:

```bash
top
```

or:

```bash
ps aux
```

to inspect CPU consumption.

For example:

```text
%CPU
```

indicates CPU usage information.

---

## 39. Memory Usage

`top` and `ps` can also show memory-related information.

Example:

```bash
ps aux
```

You may see:

```text
%MEM
RSS
VSZ
```

These represent different aspects of memory usage.

---

## 40. `/proc`

Linux exposes process and system information through:

```text
/proc
```

For example:

```bash
ls /proc
```

You may see directories such as:

```text
1
100
2000
...
```

Numeric directories commonly correspond to process IDs.

---

## 41. Inspecting a Process

Suppose the PID is:

```text
1234
```

You can inspect:

```bash
ls /proc/1234
```

You may find information related to:

```text
Command
Environment
Memory
File descriptors
Status
```

---

## 42. Process Command Line

For a process with PID 1234:

```bash
cat /proc/1234/cmdline
```

This can show the command line used to start it.

---

## 43. Process Status

Use:

```bash
cat /proc/1234/status
```

This provides detailed process information.

---

## 44. Open Files

Linux processes can have files, sockets, pipes, and other resources open.

A common tool for examining these is:

```bash
lsof
```

Example:

```bash
lsof -p 1234
```

This lists resources associated with the process where supported.

---

## 45. Process Management Workflow

When a program is causing trouble:

```text
Find process
     ↓
Get PID
     ↓
Inspect CPU / memory
     ↓
Check process state
     ↓
Try graceful termination
     ↓
Force termination only if necessary
```

Useful commands:

```bash
ps
pgrep
top
jobs
kill
lsof
```

---

## 46. Practical Example

Start:

```bash
sleep 300 &
```

Check:

```bash
jobs
```

Find it:

```bash
pgrep sleep
```

Suppose the PID is:

```text
1234
```

Inspect:

```bash
ps -p 1234 -f
```

Terminate gracefully:

```bash
kill 1234
```

Check again:

```bash
pgrep sleep
```

The process should no longer be running.

---

## 47. Practice With Background Jobs

Run:

```bash
sleep 500 &
```

Then:

```bash
jobs
```

Find the PID:

```bash
pgrep sleep
```

Inspect:

```bash
ps -p PID -f
```

Replace `PID` with the actual number.

Then terminate:

```bash
kill PID
```

---

## 48. Practice With `top`

Run:

```bash
top
```

Identify:

```text
PID
USER
%CPU
%MEM
COMMAND
```

Find a process using noticeable CPU or memory.

Do not kill an important system process just for practice.

Exit:

```text
q
```

---

## 49. Challenge

Start three background processes:

```bash
sleep 300 &
sleep 400 &
sleep 500 &
```

Run:

```bash
jobs
```

Find their PIDs:

```bash
pgrep sleep
```

Inspect them:

```bash
ps -ef | grep sleep
```

Terminate them individually:

```bash
kill PID
```

Verify:

```bash
pgrep sleep
```

---

## 50. Challenge: Process Tree

Run:

```bash
pstree
```

If available, identify:

```text
Your shell
    ↓
Commands started by your shell
```

Then run:

```bash
pstree -p
```

to include PIDs.

---

## 51. Interview Questions

### Q1. What is a process?

A running instance of a program.

### Q2. What is a PID?

A Process ID used to identify a running process.

### Q3. What is PPID?

The Process ID of a process's parent.

### Q4. What is PID 1?

The first userspace process started by the Linux system. On many modern distributions it is `systemd`.

### Q5. What does `ps` do?

It displays information about processes.

### Q6. What does `top` do?

It provides a live view of system processes and resource usage.

### Q7. What does `kill` do?

It sends a signal to a process.

### Q8. What is the difference between SIGTERM and SIGKILL?

```text
SIGTERM → requests graceful termination
SIGKILL → forces immediate termination
```

### Q9. What does `Ctrl + C` normally send?

```text
SIGINT
```

### Q10. What does `&` do?

It starts a command as a background job in the current shell.

### Q11. What does `Ctrl + Z` do?

It normally suspends the foreground process.

### Q12. What does `bg` do?

It resumes a stopped job in the background.

### Q13. What does `fg` do?

It brings a background or stopped job to the foreground.

### Q14. What is a zombie process?

A process that has terminated but whose parent has not yet collected its exit status.

### Q15. What is `/proc`?

A virtual filesystem exposing process and kernel-related information.

---

# Summary

A process is a running program.

Important concepts:

```text
Process
PID
PPID
Parent
Child
Foreground
Background
Signals
Priority
Zombie
/proc
```

Important commands:

```bash
ps
ps aux
ps -ef
pgrep
pidof
top
htop
jobs
bg
fg
kill
pkill
killall
nice
renice
pstree
lsof
```

Important keyboard shortcuts:

```text
Ctrl + C → interrupt foreground process
Ctrl + Z → suspend foreground process
```

Basic process-control workflow:

```text
Find
 ↓
Inspect
 ↓
Manage
 ↓
Terminate
```

The most important rule:

```text
Try graceful termination before force killing a process.
```

Next topic: Linux Package Management

Commit message:

```text
Add Linux process management guide
```
```
