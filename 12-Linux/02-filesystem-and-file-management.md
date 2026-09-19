# Linux Filesystem and File Management

## 1. Linux Filesystem

Linux organizes files and directories in a hierarchical structure.

Everything starts from:

```text
/
```

This is called the root directory.

A simplified filesystem looks like:

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

---

## 2. Important Directories

### `/`

The root of the entire filesystem.

```text
/
```

Everything exists somewhere below this directory.

---

### `/home`

Contains normal users' home directories.

Example:

```text
/home/aayush
```

---

### `/root`

Home directory of the root user.

Remember:

```text
/      → Filesystem root
/root  → Root user's home
```

---

### `/etc`

Contains system configuration files.

Examples:

```text
/etc/hosts
/etc/passwd
```

---

### `/var`

Contains data that changes frequently.

Common examples:

```text
/var/log
/var/cache
```

---

### `/tmp`

Used for temporary files.

Example:

```text
/tmp
```

Don't store important permanent data here.

---

### `/usr`

Contains many programs, libraries, and shared resources.

Common locations:

```text
/usr/bin
/usr/lib
/usr/share
```

---

### `/opt`

Often used for optional or third-party software.

Example:

```text
/opt/myapp
```

---

### `/dev`

Contains device files representing hardware and virtual devices.

Examples can include:

```text
/dev/sda
/dev/null
```

---

### `/proc`

A virtual filesystem containing information about running processes and the kernel.

Example:

```text
/proc/cpuinfo
```

---

## 3. Current Working Directory

Check where you are:

```bash
pwd
```

Example:

```text
/home/aayush/projects
```

This is your current working directory.

---

## 4. Listing Files

Basic:

```bash
ls
```

Detailed:

```bash
ls -l
```

Hidden files:

```bash
ls -a
```

Detailed + hidden:

```bash
ls -la
```

Human-readable sizes:

```bash
ls -lh
```

---

## 5. Understanding `ls -l`

Example:

```text
-rw-r--r-- 1 aayush users 120 Sep 19 notes.txt
```

The output contains information such as:

```text
Permissions
Links
Owner
Group
Size
Date
Name
```

---

## 6. Files vs Directories

Linux treats files and directories differently.

Example:

```text
notes.txt
```

is a file.

```text
documents/
```

is a directory.

Use:

```bash
ls -l
```

to inspect them.

A directory usually starts with:

```text
d
```

A regular file usually starts with:

```text
-
```

---

## 7. Creating Directories

Create one directory:

```bash
mkdir projects
```

Create multiple directories:

```bash
mkdir code notes backups
```

Create nested directories:

```bash
mkdir -p projects/linux/basics
```

---

## 8. Creating Files

Create an empty file:

```bash
touch notes.txt
```

Create multiple files:

```bash
touch one.txt two.txt three.txt
```

---

## 9. Writing to a File

Use `>` to create or overwrite:

```bash
echo "Hello Linux" > notes.txt
```

Check:

```bash
cat notes.txt
```

Output:

```text
Hello Linux
```

---

## 10. Appending to a File

Use `>>` to add content without replacing existing content.

Example:

```bash
echo "Second line" >> notes.txt
```

Now:

```bash
cat notes.txt
```

Output:

```text
Hello Linux
Second line
```

Remember:

```text
>   → Overwrite
>>  → Append
```

---

## 11. Reading Files With `cat`

Basic:

```bash
cat notes.txt
```

Multiple files:

```bash
cat file1.txt file2.txt
```

Number lines:

```bash
cat -n notes.txt
```

---

## 12. `less`

For large files, use:

```bash
less filename.txt
```

Useful controls:

```text
Space → Next page
b     → Previous page
↑     → Up
↓     → Down
q     → Quit
```

---

## 13. `head`

Display the beginning of a file:

```bash
head notes.txt
```

First 5 lines:

```bash
head -n 5 notes.txt
```

---

## 14. `tail`

Display the end of a file:

```bash
tail notes.txt
```

Last 5 lines:

```bash
tail -n 5 notes.txt
```

---

## 15. `tail -f`

Follow a file as it changes:

```bash
tail -f application.log
```

This is commonly useful for monitoring logs.

Exit with:

```text
Ctrl + C
```

---

## 16. Copying Files

Copy a file:

```bash
cp notes.txt backup.txt
```

Copy to a directory:

```bash
cp notes.txt backups/
```

Copy multiple files:

```bash
cp one.txt two.txt backups/
```

---

## 17. Copying Directories

Use:

```bash
cp -r projects projects-backup
```

The `-r` option means recursive copying.

It allows the command to copy directory contents.

---

## 18. Moving Files

Move:

```bash
mv notes.txt documents/
```

Rename:

```bash
mv old.txt new.txt
```

Move and rename:

```bash
mv notes.txt documents/linux-notes.txt
```

---

## 19. Removing Files

Remove a file:

```bash
rm notes.txt
```

Remove multiple files:

```bash
rm one.txt two.txt
```

Be careful before using `rm`.

---

## 20. Removing Directories

Remove an empty directory:

```bash
rmdir old-folder
```

Remove a directory and its contents:

```bash
rm -r old-folder
```

Use recursive deletion carefully.

---

## 21. Force Option

You may encounter:

```bash
rm -f file.txt
```

`-f` means force.

Combining:

```bash
rm -rf directory
```

can recursively remove a directory without normal confirmation prompts.

This command is powerful and dangerous.

Never run it blindly.

---

## 22. Absolute Paths

An absolute path starts from `/`.

Example:

```text
/home/aayush/projects/app
```

You can use it directly:

```bash
cd /home/aayush/projects/app
```

---

## 23. Relative Paths

Relative paths depend on your current location.

If you are in:

```text
/home/aayush
```

then:

```bash
cd projects
```

means:

```text
/home/aayush/projects
```

---

## 24. `.` and `..`

`.` means:

```text
Current directory
```

`..` means:

```text
Parent directory
```

Example:

```bash
cd ..
```

moves one level upward.

---

## 25. Home Shortcut

The `~` symbol represents the current user's home directory.

Example:

```bash
cd ~
```

You can also use:

```bash
ls ~
```

---

## 26. Previous Directory

The `-` argument with `cd` can take you to the previous working directory.

Example:

```bash
cd -
```

This is useful when switching between two locations.

---

## 27. Finding Files With `find`

Search for a filename:

```bash
find . -name "notes.txt"
```

Search for all `.txt` files:

```bash
find . -name "*.txt"
```

Search directories:

```bash
find . -type d -name "projects"
```

Search files:

```bash
find . -type f -name "*.log"
```

---

## 28. `locate`

Some Linux systems provide:

```bash
locate notes.txt
```

It searches an indexed database of filenames.

The database may not always be updated immediately.

---

## 29. File Information With `file`

Use:

```bash
file notes.txt
```

Example output:

```text
notes.txt: ASCII text
```

It helps identify the type of data stored in a file.

---

## 30. File Size With `du`

Check directory size:

```bash
du -sh projects
```

Example:

```text
120M    projects
```

Check files and directories:

```bash
du -h
```

---

## 31. Disk Space With `df`

Check filesystem disk usage:

```bash
df -h
```

Example:

```text
Filesystem      Size  Used Avail Use%
/dev/sda1       100G   40G   60G  40%
```

`df` shows filesystem-level disk space.

---

## 32. `du` vs `df`

Remember:

```text
du → Disk usage of files/directories

df → Free/used space of filesystems
```

---

## 33. Symbolic Links

A symbolic link is a reference to another file or directory.

Create one:

```bash
ln -s original.txt shortcut.txt
```

Now:

```text
original.txt
     ↑
     |
shortcut.txt
```

---

## 34. Hard Links

A hard link points to the same underlying file data.

Example:

```bash
ln original.txt copy.txt
```

Hard links behave differently from symbolic links and are more advanced.

For now, remember:

```text
Symbolic link → Reference/path to another file

Hard link → Another directory entry for the same file data
```

---

## 35. Checking Links

Use:

```bash
ls -l
```

A symbolic link may appear like:

```text
shortcut.txt -> original.txt
```

---

## 36. File Names

Linux filenames are case-sensitive.

These are different:

```text
Notes.txt
notes.txt
NOTES.txt
```

Don't assume they are the same file.

---

## 37. Spaces in File Names

A filename can contain spaces.

Example:

```text
my notes.txt
```

Use quotes:

```bash
cat "my notes.txt"
```

Or escape the space:

```bash
cat my\ notes.txt
```

---

## 38. Special Characters

Special characters can have meaning in the shell.

Examples:

```text
*
?
>
<
|
$
&
;
```

Be careful when using them in filenames or commands.

---

## 39. Wildcards

The `*` wildcard can match multiple characters.

Example:

```bash
ls *.txt
```

This can list:

```text
notes.txt
readme.txt
data.txt
```

The `?` wildcard can match a single character.

Example:

```bash
ls file?.txt
```

---

## 40. File Management Example

Create:

```bash
mkdir -p linux-practice/files
cd linux-practice/files
```

Create files:

```bash
touch one.txt two.txt three.txt
```

Write content:

```bash
echo "Linux" > one.txt
echo "Commands" > two.txt
echo "Practice" > three.txt
```

List:

```bash
ls -l
```

Read:

```bash
cat one.txt
```

Copy:

```bash
cp one.txt one-backup.txt
```

Rename:

```bash
mv two.txt linux-commands.txt
```

Then:

```bash
ls -l
```

---

## 41. Practice

Create this structure:

```text
linux-files/
├── documents/
│   ├── notes.txt
│   └── commands.txt
│
├── backups/
│
└── projects/
```

Commands:

```bash
mkdir -p linux-files/documents
mkdir -p linux-files/backups
mkdir -p linux-files/projects

touch linux-files/documents/notes.txt
touch linux-files/documents/commands.txt
```

Add content:

```bash
echo "Linux file management" > linux-files/documents/notes.txt
echo "pwd ls cd mkdir cp mv rm" > linux-files/documents/commands.txt
```

Copy:

```bash
cp linux-files/documents/notes.txt linux-files/backups/
```

Verify:

```bash
find linux-files -type f
```

---

## 42. Challenge

Create:

```text
linux-project/
├── src/
│   ├── app.txt
│   └── config.txt
│
├── docs/
│   └── README.txt
│
├── backup/
│
└── logs/
```

Then:

1. Add content to every file.
2. Copy `src/app.txt` to `backup/`.
3. Rename `config.txt` to `application-config.txt`.
4. Move `README.txt` into `src/`.
5. Find all `.txt` files.
6. Display the size of `linux-project`.
7. Display filesystem disk space.

Useful commands:

```bash
find linux-project -type f
du -sh linux-project
df -h
```

---

## 43. Important Commands

```text
pwd
ls
cd
mkdir
touch
cat
less
head
tail
cp
mv
rm
rmdir
find
file
du
df
ln
```

---

## 44. Quick Reference

| Command | Purpose |
|---|---|
| `pwd` | Current directory |
| `ls` | List files |
| `cd` | Change directory |
| `mkdir` | Create directory |
| `touch` | Create file |
| `cat` | Read file |
| `less` | Read large file |
| `head` | Beginning of file |
| `tail` | End of file |
| `cp` | Copy |
| `mv` | Move/rename |
| `rm` | Delete |
| `rmdir` | Remove empty directory |
| `find` | Search files |
| `file` | Identify file type |
| `du` | Disk usage |
| `df` | Filesystem space |
| `ln` | Create links |

---

## 45. Interview Questions

### Q1. What is the Linux root directory?

```text
/
```

It is the top-level directory of the Linux filesystem.

### Q2. What is the difference between an absolute and relative path?

An absolute path starts from `/`, while a relative path starts from the current working directory.

### Q3. What does `mkdir -p` do?

It creates directories along with required parent directories.

### Q4. What is the difference between `>` and `>>`?

```text
>   → Overwrites
>>  → Appends
```

### Q5. What does `cp -r` do?

It recursively copies a directory and its contents.

### Q6. What is the difference between `cp` and `mv`?

`cp` creates a copy, while `mv` moves or renames the original.

### Q7. What is the difference between `du` and `df`?

`du` reports disk usage by files/directories, while `df` reports filesystem space usage.

### Q8. What does `find` do?

It searches for files and directories based on specified conditions.

### Q9. What is a symbolic link?

A symbolic link is a filesystem entry that points to another file or directory.

### Q10. Why should `rm -rf` be used carefully?

It can recursively and forcefully remove directories and their contents, potentially causing significant data loss.

---

# Summary

Linux uses a hierarchical filesystem beginning at:

```text
/
```

You should understand these important directories:

```text
/home
/root
/etc
/var
/tmp
/usr
/opt
/dev
/proc
```

You should also be comfortable with:

```text
Absolute paths
Relative paths
Files
Directories
Hidden files
Wildcards
Symbolic links
Disk usage
File searching
```

Most importantly, master:

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
find
du
df
```

The basic file-management workflow is:

```text
Navigate
   ↓
Create
   ↓
Read
   ↓
Copy
   ↓
Move
   ↓
Search
   ↓
Delete
```

Next topic: Linux File Permissions and Ownership 
