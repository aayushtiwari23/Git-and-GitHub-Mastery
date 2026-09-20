
# Linux File Permissions and Ownership

## 1. Why File Permissions Matter

Linux is a multi-user operating system.

Different users may need different levels of access to files and directories.

Linux permissions control:

```text
Who can access something
What they can do
Whether they can execute it
```

The three basic permissions are:

```text
r → Read
w → Write
x → Execute
```

---

## 2. Three Permission Categories

Linux permissions are assigned to three categories:

```text
User
Group
Others
```

### User

The owner of the file.

### Group

Users belonging to the file's group.

### Others

Everyone else.

---

## 3. Viewing Permissions

Run:

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 aayush users 120 Sep 20 notes.txt
```

The first part:

```text
-rw-r--r--
```

contains the permissions.

---

## 4. Understanding Permission Structure

Consider:

```text
-rw-r--r--
```

Break it into:

```text
- rw- r-- r--
  │   │   │
  │   │   └── Others
  │   └────── Group
  └────────── User
```

The first character indicates the file type.

```text
- → Regular file
d → Directory
l → Symbolic link
```

---

## 5. Read Permission

```text
r
```

Read allows you to view the contents of a file.

For example:

```bash
cat notes.txt
```

requires appropriate read permission.

Numeric value:

```text
r = 4
```

---

## 6. Write Permission

```text
w
```

Write allows modification of a file.

Numeric value:

```text
w = 2
```

---

## 7. Execute Permission

```text
x
```

Execute allows a file to be executed as a program or script when the system and other permissions permit it.

Numeric value:

```text
x = 1
```

---

## 8. Permission Values

Remember:

```text
r = 4
w = 2
x = 1
```

Therefore:

```text
r-- = 4
-w- = 2
--x = 1
```

Combining permissions:

```text
rw- = 4 + 2 = 6
r-x = 4 + 1 = 5
-wx = 2 + 1 = 3
rwx = 4 + 2 + 1 = 7
```

---

## 9. Numeric Permissions

The three permission groups each receive a number.

Example:

```text
755
```

means:

```text
7 → User
5 → Group
5 → Others
```

And:

```text
7 = rwx
5 = r-x
5 = r-x
```

Therefore:

```text
755 → rwxr-xr-x
```

---

## 10. Common Permission Values

```text
644 → rw-r--r--
755 → rwxr-xr-x
600 → rw-------
700 → rwx------
```

These are common patterns, but the correct permissions depend on the situation.

---

## 11. `chmod`

`chmod` changes permissions.

Example:

```bash
chmod 755 script.sh
```

This gives:

```text
User   → rwx
Group  → r-x
Others → r-x
```

---

## 12. Making a Script Executable

Suppose:

```text
script.sh
```

does not have execute permission.

Run:

```bash
chmod +x script.sh
```

Then:

```bash
./script.sh
```

can execute it if the other required conditions are satisfied.

---

## 13. Symbolic `chmod`

Instead of numbers, you can use letters.

Give the user execute permission:

```bash
chmod u+x script.sh
```

Remove user execute permission:

```bash
chmod u-x script.sh
```

Give group write permission:

```bash
chmod g+w file.txt
```

Remove group write permission:

```bash
chmod g-w file.txt
```

Give others read permission:

```bash
chmod o+r file.txt
```

---

## 14. Permission Categories in `chmod`

```text
u → User
g → Group
o → Others
a → All
```

Examples:

```bash
chmod u+x script.sh
chmod g+r file.txt
chmod o-r file.txt
chmod a+x script.sh
```

---

## 15. `chmod` Operators

```text
+ → Add permission
- → Remove permission
= → Set exact permission
```

Examples:

```bash
chmod u+x script.sh
chmod g-w file.txt
chmod o=r file.txt
```

---

## 16. Exact Symbolic Permissions

Example:

```bash
chmod u=rwx,g=rx,o=r file.txt
```

This sets:

```text
User   → rwx
Group  → r-x
Others → r--
```

Equivalent numeric permission:

```text
754
```

---

## 17. Recursive Permissions

To change permissions for a directory and its contents:

```bash
chmod -R 755 project/
```

`-R` means recursive.

Use recursive permission changes carefully.

You may accidentally give permissions to files that should have different settings.

---

## 18. File Ownership

Every file has an owner and a group.

Example:

```bash
ls -l
```

Output:

```text
-rw-r--r-- 1 aayush developers 120 notes.txt
```

Here:

```text
aayush     → Owner
developers → Group
```

---

## 19. `whoami`

Check your current username:

```bash
whoami
```

Example:

```text
aayush
```

---

## 20. `id`

View your user and group information:

```bash
id
```

Example:

```text
uid=1000(aayush) gid=1000(aayush) groups=1000(aayush),27(sudo)
```

This shows information such as:

```text
User ID
Group ID
Groups
```

---

## 21. `groups`

Display groups associated with your user:

```bash
groups
```

Example:

```text
aayush sudo developers
```

---

## 22. Changing Ownership

`chown` changes ownership.

Example:

```bash
sudo chown user file.txt
```

Change owner and group:

```bash
sudo chown user:developers file.txt
```

Use `sudo` only when the operation requires elevated privileges.

---

## 23. Changing Group Ownership

`chgrp` changes the group ownership.

Example:

```bash
sudo chgrp developers file.txt
```

Now the file belongs to the specified group.

---

## 24. Checking Ownership

Run:

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 aayush developers 120 notes.txt
```

The owner and group appear after the link count.

---

## 25. Root User

Linux has a special administrative account called:

```text
root
```

Root has very powerful privileges.

Root can generally:

```text
Modify system files
Install software
Change ownership
Change permissions
Manage users
Manage services
```

Because root can make system-wide changes, it should be used carefully.

---

## 26. `sudo`

`sudo` allows an authorized user to execute a command with elevated privileges.

Example:

```bash
sudo apt update
```

You may be asked for your password.

---

## 27. Why `sudo` Should Be Used Carefully

Avoid blindly running:

```bash
sudo ...
```

Every command should be understood before giving it elevated privileges.

Especially be careful with commands involving:

```text
rm
chmod
chown
system configuration
disk operations
```

---

## 28. Directory Permissions

Permissions behave differently for directories.

For a directory:

```text
r → List directory contents
w → Create/delete/rename entries
x → Enter/traverse the directory
```

This is extremely important.

---

## 29. Directory Read Permission

If you have:

```text
r
```

on a directory, you can generally list its entries.

For example:

```bash
ls directory/
```

---

## 30. Directory Write Permission

Write permission allows modification of directory entries.

Depending on other permissions, this can allow:

```text
Create files
Delete files
Rename files
```

---

## 31. Directory Execute Permission

Execute permission on a directory allows you to traverse it.

For example:

```bash
cd directory/
```

requires appropriate execute permission.

---

## 32. File vs Directory Permissions

Remember:

```text
File:

r → Read contents
w → Modify contents
x → Execute

Directory:

r → List entries
w → Modify entries
x → Enter/traverse
```

---

## 33. `umask`

`umask` controls default permission restrictions when new files and directories are created.

Check it:

```bash
umask
```

Example:

```text
022
```

The exact resulting permissions depend on the program creating the file and the system's defaults.

---

## 34. Default Permissions

A common conceptual model is:

```text
Files:
666 minus umask

Directories:
777 minus umask
```

For example, with:

```text
umask = 022
```

a typical new file may start with:

```text
644
```

and a typical new directory may start with:

```text
755
```

The actual behavior can vary depending on the application and system.

---

## 35. Special Permissions

Linux also supports special permission bits:

```text
SUID
SGID
Sticky Bit
```

These are more advanced permissions.

---

## 36. SUID

SUID stands for:

```text
Set User ID
```

When applied to an executable, it can cause the program to run with the effective privileges of the file owner.

Example representation:

```text
-rwsr-xr-x
```

SUID should be used carefully because it can have security implications.

---

## 37. SGID

SGID stands for:

```text
Set Group ID
```

It has different behavior depending on whether it is applied to a file or directory.

On directories, SGID commonly causes newly created files to inherit the directory's group.

Example:

```text
drwxrwsr-x
```

---

## 38. Sticky Bit

The sticky bit is commonly used on shared directories.

Example:

```text
drwxrwxrwt
```

A classic example is:

```text
/tmp
```

The sticky bit helps prevent users from deleting or renaming files owned by other users in a shared writable directory, subject to system privileges.

---

## 39. Permission Example

Consider:

```text
-rwxr-x---
```

Break it down:

```text
User   → rwx
Group  → r-x
Others → ---
```

Numeric form:

```text
750
```

---

## 40. Another Example

Consider:

```text
-rw-r-----
```

Permissions:

```text
User   → rw-
Group  → r--
Others → ---
```

Numeric form:

```text
640
```

---

## 41. Practical Example

Create a file:

```bash
touch secret.txt
```

Check:

```bash
ls -l secret.txt
```

Change permissions:

```bash
chmod 600 secret.txt
```

Check again:

```bash
ls -l secret.txt
```

You should see permissions similar to:

```text
-rw-------
```

---

## 42. Script Example

Create:

```bash
touch script.sh
```

Add:

```bash
echo '#!/bin/bash' > script.sh
echo 'echo "Linux script"' >> script.sh
```

Check:

```bash
ls -l script.sh
```

Make executable:

```bash
chmod +x script.sh
```

Run:

```bash
./script.sh
```

---

## 43. Permission Practice

Create:

```text
permissions-practice/
├── public.txt
├── private.txt
└── script.sh
```

Commands:

```bash
mkdir permissions-practice
cd permissions-practice

touch public.txt private.txt script.sh
```

Set:

```bash
chmod 644 public.txt
chmod 600 private.txt
chmod 755 script.sh
```

Check:

```bash
ls -l
```

Expected permission patterns:

```text
public.txt  → rw-r--r--
private.txt → rw-------
script.sh   → rwxr-xr-x
```

---

## 44. Ownership Practice

Check:

```bash
whoami
id
groups
```

Then:

```bash
ls -l
```

Identify:

```text
Owner
Group
Permissions
```

Do not change ownership of system files during practice.

---

## 45. Challenge

Create:

```text
secure-project/
├── public/
├── private/
└── scripts/
```

Create:

```text
public/readme.txt
private/secret.txt
scripts/run.sh
```

Set:

```text
readme.txt → 644
secret.txt → 600
run.sh     → 755
```

Then verify:

```bash
ls -l public
ls -l private
ls -l scripts
```

---

## 46. Security Challenge

Create:

```text
secure-project/config.txt
```

Set:

```bash
chmod 600 secure-project/config.txt
```

Verify:

```bash
ls -l secure-project/config.txt
```

Explain why configuration files containing sensitive information should generally not be world-readable.

---

## 47. Important Commands

```text
ls -l
chmod
chown
chgrp
whoami
id
groups
sudo
umask
```

---

## 48. Quick Reference

| Command | Purpose |
|---|---|
| `ls -l` | View permissions and ownership |
| `chmod` | Change permissions |
| `chown` | Change owner |
| `chgrp` | Change group |
| `whoami` | Show current user |
| `id` | Show user/group IDs |
| `groups` | Show user's groups |
| `sudo` | Run command with elevated privileges |
| `umask` | Show/change permission mask |

---

## 49. Permission Cheat Sheet

```text
r = 4
w = 2
x = 1
```

Common values:

```text
7 = rwx
6 = rw-
5 = r-x
4 = r--
3 = -wx
2 = -w-
1 = --x
0 = ---
```

Common combinations:

```text
600 → rw-------
644 → rw-r--r--
640 → rw-r-----
700 → rwx------
750 → rwxr-x---
755 → rwxr-xr-x
```

---

## 50. Interview Questions

### Q1. What are the three basic Linux permissions?

```text
Read
Write
Execute
```

### Q2. What are the three permission categories?

```text
User
Group
Others
```

### Q3. What does `chmod 755` mean?

```text
User   → rwx
Group  → r-x
Others → r-x
```

### Q4. What does `chmod 644` mean?

```text
User   → rw-
Group  → r--
Others → r--
```

### Q5. What does `chmod +x` do?

It adds execute permission according to the command's default target, commonly making a script executable.

### Q6. What does `chown` do?

It changes the owner and optionally the group of a file or directory.

### Q7. What does `sudo` do?

It allows an authorized user to run a command with elevated privileges.

### Q8. What does execute permission mean for a directory?

It allows traversal or entering the directory, subject to the other required permissions.

### Q9. What is the difference between `600` and `644`?

```text
600 → Owner can read/write; group and others have no permissions.

644 → Owner can read/write; group and others can read.
```

### Q10. Why are permissions important?

They help prevent unauthorized access, modification, and execution of files and directories.

---

# Summary

Linux permissions control access to files and directories.

The three basic permissions are:

```text
r → Read
w → Write
x → Execute
```

They apply to:

```text
User
Group
Others
```

The most important command is:

```bash
chmod
```

Ownership is managed with:

```bash
chown
chgrp
```

User information can be inspected with:

```bash
whoami
id
groups
```

Administrative privileges can be requested with:

```bash
sudo
```

The key concept to remember is:

```text
          User    Group   Others
            ↓       ↓       ↓
Permissions rwx     r-x     r--
            ↓       ↓       ↓
Numeric       7       5       4
```

So:

```text
754 = rwxr-xr--
```

Next topic: Linux Users, Groups, and User Management

Commit message:

```text
Add Linux file permissions and ownership guide
```
```
