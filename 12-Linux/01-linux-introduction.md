# Linux Introduction
## 1. What Is Linux?

Linux is an open-source operating system kernel.

A complete Linux operating system is usually called a Linux distribution or distro.

Examples:

```text
Ubuntu
Debian
Fedora
Arch Linux
Linux Mint
Kali Linux
Red Hat Enterprise Linux
```

Linux is widely used in:

- Servers
- Cloud computing
- DevOps
- Cybersecurity
- Software development
- Containers
- Networking
- Embedded systems
- Supercomputers

---

## 2. Linux vs Windows

Linux and Windows are both operating-system platforms, but they work differently.

| Feature | Linux | Windows |
|---|---|---|
| Source | Mostly open source | Mostly proprietary |
| Terminal | Very important | Available |
| Customization | Very high | Moderate |
| Servers | Very common | Very common |
| Development | Very popular | Very popular |
| Package management | Distribution-dependent | Microsoft Store / installers / package managers |
| File system | Unix-like hierarchy | Drive-based structure |

---

## 3. Linux Kernel

The kernel is the core component of the operating system.

It manages:

```text
CPU
Memory
Processes
Devices
File systems
Networking
Security
```

Conceptually:

```text
Applications
     ↓
Shell / System Libraries
     ↓
Linux Kernel
     ↓
Hardware
```

---

## 4. Linux Distribution

A Linux distribution combines the Linux kernel with additional software.

For example:

```text
Linux Kernel
     +
Package Manager
     +
System Utilities
     +
Desktop Environment
     +
Applications
     ↓
Linux Distribution
```

Ubuntu is an example of a Linux distribution.

---

## 5. Ubuntu

Ubuntu is one of the most widely used Linux distributions.

It is commonly used for:

- Learning Linux
- Programming
- Servers
- Cloud computing
- DevOps
- Development environments

For beginners, Ubuntu is a good distribution to learn Linux fundamentals.

---

## 6. Linux Terminal

The terminal is a text-based interface used to interact with the operating system.

Example:

```bash
pwd
```

The terminal allows you to:

```text
Navigate files
Create files
Delete files
Install software
Manage processes
Inspect systems
Configure networks
Automate tasks
```

---

## 7. Shell

A shell is a program that interprets commands and communicates with the operating system.

Common shells include:

```text
Bash
Zsh
Fish
PowerShell
```

Bash is one of the most commonly encountered shells on Linux.

---

## 8. Terminal vs Shell

These terms are related but different.

### Terminal

The application/interface where you type commands.

### Shell

The program that interprets those commands.

Conceptually:

```text
You
 ↓
Terminal
 ↓
Shell
 ↓
Linux
 ↓
Hardware
```

---

## 9. Your First Linux Command

Run:

```bash
echo "Hello Linux"
```

Output:

```text
Hello Linux
```

`echo` prints text to the terminal.

---

## 10. `pwd`

`pwd` means:

```text
Print Working Directory
```

Run:

```bash
pwd
```

Example output:

```text
/home/aayush
```

It tells you where you currently are.

---

## 11. `ls`

`ls` lists files and directories.

Run:

```bash
ls
```

Example:

```text
Documents
Downloads
Pictures
Projects
```

---

## 12. Useful `ls` Options

Detailed listing:

```bash
ls -l
```

Show hidden files:

```bash
ls -a
```

Detailed listing including hidden files:

```bash
ls -la
```

Human-readable file sizes:

```bash
ls -lh
```

---

## 13. Hidden Files

Linux files beginning with `.` are commonly hidden.

Example:

```text
.bashrc
.gitconfig
```

Use:

```bash
ls -a
```

to see them.

---

## 14. `cd`

`cd` means:

```text
Change Directory
```

Example:

```bash
cd Documents
```

Move into the Documents directory.

Go back one directory:

```bash
cd ..
```

Go to your home directory:

```bash
cd ~
```

---

## 15. Absolute Paths

An absolute path starts from the root directory.

Example:

```text
/home/aayush/Documents/project
```

It specifies the complete location.

---

## 16. Relative Paths

A relative path starts from your current directory.

Example:

```text
Documents/project
```

If you are already in:

```text
/home/aayush
```

then:

```text
Documents/project
```

refers to:

```text
/home/aayush/Documents/project
```

---

## 17. Root Directory

Linux has one main root directory:

```text
/
```

This is different from the Windows concept of:

```text
C:\
D:\
```

Linux starts its filesystem hierarchy from:

```text
/
```

---

## 18. Important Linux Directories

Common directories include:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── opt
├── proc
├── root
├── run
├── tmp
├── usr
└── var
```

You don't need to memorize everything immediately.

Start with:

```text
/
├── home
├── etc
├── usr
├── var
├── tmp
└── root
```

---

## 19. `/home`

Normal users generally have their personal directories under:

```text
/home
```

Example:

```text
/home/aayush
```

This is commonly your user home directory.

---

## 20. `/root`

`/root` is the home directory of the root user.

It is different from:

```text
/
```

Remember:

```text
/       → Root directory

/root   → Root user's home directory
```

---

## 21. `/etc`

`/etc` contains many system configuration files.

Examples include configuration for:

```text
Networking
Users
Services
System settings
```

Don't randomly modify files in `/etc`.

---

## 22. `/tmp`

`/tmp` is commonly used for temporary files.

Applications and users may create temporary data there.

Don't store important permanent files there.

---

## 23. `/var`

`/var` contains variable data.

Common examples include:

```text
Logs
Caches
Application data
Spools
```

System logs are often found under:

```text
/var/log
```

---

## 24. `/usr`

`/usr` contains many user-space programs, libraries, and related resources.

You may encounter directories such as:

```text
/usr/bin
/usr/lib
/usr/share
```

---

## 25. `mkdir`

`mkdir` creates a directory.

Example:

```bash
mkdir linux-practice
```

Create nested directories:

```bash
mkdir -p projects/linux/basics
```

---

## 26. `touch`

`touch` can create an empty file.

Example:

```bash
touch notes.txt
```

Now:

```bash
ls
```

may show:

```text
notes.txt
```

---

## 27. `cat`

`cat` can display file contents.

Example:

```bash
cat notes.txt
```

You can also create simple content:

```bash
echo "Linux is powerful" > notes.txt
```

Then:

```bash
cat notes.txt
```

Output:

```text
Linux is powerful
```

---

## 28. `cp`

`cp` copies files or directories.

Example:

```bash
cp notes.txt backup.txt
```

Now there are two files:

```text
notes.txt
backup.txt
```

---

## 29. `mv`

`mv` moves or renames files.

Rename:

```bash
mv notes.txt linux-notes.txt
```

Move:

```bash
mv linux-notes.txt Documents/
```

---

## 30. `rm`

`rm` removes files.

Example:

```bash
rm backup.txt
```

Be careful with `rm`.

Unlike a graphical recycle bin, command-line deletion can be immediate.

---

## 31. `rmdir`

`rmdir` removes an empty directory.

Example:

```bash
rmdir old-folder
```

It will not normally remove a directory containing files.

---

## 32. Command Help

Many Linux commands support:

```bash
--help
```

Example:

```bash
ls --help
```

This provides information about available options.

---

## 33. `man`

`man` means manual.

Example:

```bash
man ls
```

It opens the manual page for `ls`.

You can search documentation for commands using:

```bash
man mkdir
man cp
man mv
```

---

## 34. Command History

Use:

```bash
history
```

to view previously executed commands.

You can also press the:

```text
↑
```

arrow key to recall previous commands.

---

## 35. Clearing the Terminal

Use:

```bash
clear
```

This clears the visible terminal screen.

---

## 36. Tab Completion

Terminal shells support tab completion.

For example, type:

```bash
cd Doc
```

and press:

```text
Tab
```

The shell may complete:

```bash
cd Documents
```

This saves time and reduces typing mistakes.

---

## 37. Command Structure

A command commonly follows this pattern:

```text
command + options + arguments
```

Example:

```bash
ls -l Documents
```

Here:

```text
ls          → command
-l          → option
Documents   → argument
```

---

## 38. Practice

Open a Linux terminal and run:

```bash
pwd
ls
ls -la
mkdir linux-practice
cd linux-practice
touch notes.txt
echo "I am learning Linux" > notes.txt
cat notes.txt
pwd
ls -l
```

Then return:

```bash
cd ..
```

---

## 39. Challenge

Create this directory structure:

```text
linux-practice/
├── commands/
├── notes/
└── projects/
```

Inside `notes`, create:

```text
linux.txt
```

Put this inside:

```text
I am learning Linux command line fundamentals.
```

Then verify everything using:

```bash
ls
ls commands
ls notes
cat notes/linux.txt
ls projects
```

---

## 40. Important Commands

Remember these first:

```text
pwd      → Show current directory
ls       → List files
cd       → Change directory
mkdir    → Create directory
touch    → Create file
cat      → Display file
cp       → Copy
mv       → Move/rename
rm       → Remove
rmdir    → Remove empty directory
clear    → Clear terminal
history   → Show command history
man      → Manual
```

---

## 41. Interview Questions

### Q1. What is Linux?

Linux is an open-source operating system kernel used in many operating systems and computing environments.

### Q2. What is a Linux distribution?

A distribution packages the Linux kernel with system software, package management, and other components.

### Q3. What is the Linux root directory?

```text
/
```

It is the top of the Linux filesystem hierarchy.

### Q4. What does `pwd` do?

It displays the current working directory.

### Q5. What does `ls` do?

It lists files and directories.

### Q6. What does `cd` do?

It changes the current working directory.

### Q7. What is the difference between `/` and `/root`?

`/` is the root of the filesystem. `/root` is the home directory of the root user.

### Q8. What is Bash?

Bash is a command-line shell commonly used on Linux systems.

### Q9. What is the difference between a terminal and a shell?

A terminal provides the interface, while the shell interprets commands.

### Q10. Why should `rm` be used carefully?

Because deleting files from the command line can be immediate and potentially difficult to recover.

---

# Summary

Linux is fundamental to:

```text
Cloud
DevOps
Servers
Cybersecurity
Programming
Containers
Networking
```

The most important concepts from this lesson are:

```text
Linux
Kernel
Distribution
Terminal
Shell
Filesystem
Root directory
Home directory
Absolute path
Relative path
```

The first commands to master are:

```bash
pwd
ls
cd
mkdir
touch
cat
cp
mv
rm
man
```

Your basic command-line workflow is:

```text
Navigate
   ↓
Create
   ↓
Read
   ↓
Modify
   ↓
Move
   ↓
Remove
```

Next topic: Linux Filesystem and File Management
