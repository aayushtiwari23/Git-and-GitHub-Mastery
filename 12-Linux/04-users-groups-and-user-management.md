# Linux Users, Groups, and User Management

## 1. Why Users and Groups Matter

Linux is designed to support multiple users.

Each user can have:

- A username
- A user ID
- A home directory
- A primary group
- Additional groups
- Permissions

Groups make it easier to manage permissions for multiple users.

Basic structure:

```text
User
 ↓
Primary Group
 ↓
Additional Groups
 ↓
Permissions
```

---

## 2. Current User

Check the current user:

```bash
whoami
```

Example:

```text
aayush
```

This tells you which user is currently running the shell.

---

## 3. User Information

Use:

```bash
id
```

Example:

```text
uid=1000(aayush) gid=1000(aayush) groups=1000(aayush),27(sudo)
```

This can show:

```text
UID
Username
GID
Primary group
Additional groups
```

---

## 4. UID

UID means:

```text
User ID
```

Linux internally identifies users using numeric IDs.

Example:

```text
uid=1000(aayush)
```

Here:

```text
1000 → UID
aayush → Username
```

---

## 5. GID

GID means:

```text
Group ID
```

Example:

```text
gid=1000(aayush)
```

Here:

```text
1000 → GID
aayush → Group name
```

---

## 6. Root User

The administrative account is:

```text
root
```

Root normally has UID:

```text
0
```

Root has extensive privileges over the system.

Check:

```bash
id root
```

You may see:

```text
uid=0(root)
```

---

## 7. Regular Users

Normal users generally have non-zero UIDs.

On many modern Linux systems, regular users commonly start around UID 1000, but the exact ranges depend on the distribution and configuration.

Example:

```text
aayush → UID 1000
```

---

## 8. Home Directory

A user's personal files are commonly stored under:

```text
/home
```

Example:

```text
/home/aayush
```

Check your home directory:

```bash
echo $HOME
```

You can also use:

```bash
cd ~
```

---

## 9. Environment Variable `$USER`

Check the current username:

```bash
echo $USER
```

Example:

```text
aayush
```

Useful variables include:

```text
$USER
$HOME
$PATH
$SHELL
```

---

## 10. `/etc/passwd`

Linux stores basic user-account information in:

```text
/etc/passwd
```

View it:

```bash
cat /etc/passwd
```

A typical entry looks like:

```text
aayush:x:1000:1000:Aayush:/home/aayush:/bin/bash
```

The fields are separated by:

```text
:
```

---

## 11. `/etc/passwd` Fields

A simplified structure is:

```text
username:password-placeholder:UID:GID:GECOS:home:shell
```

Example:

```text
aayush:x:1000:1000:Aayush:/home/aayush:/bin/bash
```

Meaning:

```text
Username
Password placeholder
UID
GID
User information
Home directory
Login shell
```

---

## 12. Password Information

Modern Linux systems do not normally store actual password hashes directly in `/etc/passwd`.

Password-related information is generally stored in:

```text
/etc/shadow
```

Access to `/etc/shadow` is restricted.

Do not modify these files manually unless you know exactly what you are doing.

---

## 13. `/etc/shadow`

You may see entries similar to:

```text
username:$hash:...
```

The file contains password-related authentication information and account settings.

Check permissions:

```bash
ls -l /etc/shadow
```

Access is normally restricted to privileged users.

---

## 14. Listing Users

One way to inspect users is:

```bash
cat /etc/passwd
```

To display usernames only:

```bash
cut -d: -f1 /etc/passwd
```

This extracts the first field from each entry.

---

## 15. `getent passwd`

A more flexible way to query user information is:

```bash
getent passwd
```

For a specific user:

```bash
getent passwd aayush
```

This can work with local and configured directory services.

---

## 16. Groups

Groups allow multiple users to share permissions.

Check your groups:

```bash
groups
```

Example:

```text
aayush sudo developers
```

This means the user belongs to groups such as:

```text
aayush
sudo
developers
```

---

## 17. Primary Group

Every user has a primary group.

Check:

```bash
id
```

Example:

```text
gid=1000(aayush)
```

The group shown after `gid=` is the primary group.

---

## 18. Secondary Groups

Users can also belong to additional groups.

Example:

```text
groups=aayush,sudo,docker,developers
```

These are additional group memberships.

Groups can be used to grant access to shared resources.

---

## 19. `/etc/group`

Group information is commonly stored in:

```text
/etc/group
```

View it:

```bash
cat /etc/group
```

A simplified entry may look like:

```text
developers:x:1001:aayush,user2
```

Fields include:

```text
Group name
Password placeholder
GID
Group members
```

---

## 20. Creating a User

The `useradd` command can create users.

Example:

```bash
sudo useradd newuser
```

This creates a user account.

However, the exact home-directory and shell behavior can depend on options and distribution defaults.

---

## 21. Creating a User With Home Directory

A common approach is:

```bash
sudo useradd -m newuser
```

`-m` requests creation of a home directory.

Example:

```text
/home/newuser
```

---

## 22. Setting a Password

Use:

```bash
sudo passwd newuser
```

The system will ask you to enter and confirm a password.

Never share passwords in repositories, scripts, screenshots, or chat messages.

---

## 23. Creating a User With a Login Shell

Example:

```bash
sudo useradd -m -s /bin/bash newuser
```

This requests:

```text
Home directory → /home/newuser
Shell → /bin/bash
```

---

## 24. `adduser`

Some Linux distributions provide:

```bash
sudo adduser newuser
```

It provides an interactive interface for creating a user.

The availability and exact behavior depend on the Linux distribution.

---

## 25. Switching Users

Use:

```bash
su - newuser
```

This starts a login shell as another user if you have the required credentials/privileges.

Return to the previous user:

```bash
exit
```

---

## 26. Using `sudo` as Another User

You can run a command as another user:

```bash
sudo -u newuser whoami
```

Expected output:

```text
newuser
```

This is useful for testing permissions and application behavior.

---

## 27. Creating a Group

Use:

```bash
sudo groupadd developers
```

This creates a group named:

```text
developers
```

---

## 28. Adding a User to a Group

A common command is:

```bash
sudo usermod -aG developers newuser
```

Important:

```text
-a → Append
-G → Supplementary groups
```

The `-a` is important because omitting it can replace the user's existing supplementary group list.

---

## 29. Verify Group Membership

Run:

```bash
groups newuser
```

Or:

```bash
id newuser
```

You should see the new group.

---

## 30. Removing a User From a Group

On systems using GNU `usermod`, one approach is:

```bash
sudo gpasswd -d newuser developers
```

Then verify:

```bash
groups newuser
```

---

## 31. Changing a User's Primary Group

Use:

```bash
sudo usermod -g developers newuser
```

This changes the user's primary group.

Use this carefully because changing the primary group can affect ownership and application behavior.

---

## 32. Changing a Username

Use:

```bash
sudo usermod -l newname oldname
```

This changes the login name.

Changing usernames can have consequences for:

```text
Home directories
File ownership
Applications
Configuration
Scripts
```

Do not perform this on an important account without understanding the consequences.

---

## 33. Changing a Home Directory

A common command is:

```bash
sudo usermod -d /home/newhome -m newuser
```

This changes the configured home directory and requests moving existing contents.

Use carefully.

---

## 34. Locking a User Account

To lock a user's password:

```bash
sudo passwd -l newuser
```

This can prevent password-based authentication for the account.

It does not necessarily disable every possible authentication method.

---

## 35. Unlocking a User Account

Use:

```bash
sudo passwd -u newuser
```

This unlocks the password.

---

## 36. Deleting a User

Remove a user account:

```bash
sudo userdel newuser
```

This does not necessarily remove the user's home directory.

---

## 37. Removing a User and Home Directory

On systems supporting the option:

```bash
sudo userdel -r newuser
```

This requests removal of the user's home directory and mail spool along with the account.

Use carefully.

---

## 38. Group Deletion

Delete a group:

```bash
sudo groupdel developers
```

Do this only after confirming that the group is no longer needed.

---

## 39. Checking Logged-In Users

Use:

```bash
who
```

This shows users currently logged in.

Another command:

```bash
w
```

provides additional information about logged-in users and their activity.

---

## 40. `last`

View login history:

```bash
last
```

This can show previous login sessions depending on system configuration.

---

## 41. `whoami` vs `who`

Remember:

```text
whoami → Which user am I?
who    → Who is currently logged in?
```

---

## 42. `id` vs `groups`

```text
id
```

provides detailed user and group information.

```text
groups
```

focuses mainly on group membership.

---

## 43. User and Group Relationship

Conceptually:

```text
User
 │
 ├── UID
 │
 ├── Home Directory
 │
 ├── Login Shell
 │
 ├── Primary Group
 │
 └── Secondary Groups
```

---

## 44. Groups and Permissions

Suppose:

```text
project/
└── data.txt
```

Ownership:

```text
Owner  → aayush
Group  → developers
```

Permissions:

```text
-rw-rw----
```

This means:

```text
Owner      → Read + Write
Group      → Read + Write
Others     → No access
```

Anyone in the `developers` group can potentially modify the file, assuming there are no other restrictions.

---

## 45. Shared Project Directory

A shared development directory can use a group.

Example:

```text
developers
```

Users:

```text
aayush
user1
user2
```

Conceptually:

```text
developers
     │
 ┌───┼───┐
 ↓   ↓   ↓
aayush user1 user2
     │
     ↓
Shared Project
```

This is one of the main reasons Linux groups are useful.

---

## 46. `sudo` Group

Many distributions have a group that grants administrative privileges through `sudo`.

On Ubuntu, this group is commonly:

```text
sudo
```

Check:

```bash
groups
```

You may see:

```text
sudo
```

Other distributions can use different administrative groups, such as:

```text
wheel
```

Do not assume every Linux distribution uses the same configuration.

---

## 47. Adding a User to the `sudo` Group

On Ubuntu-like systems:

```bash
sudo usermod -aG sudo newuser
```

On systems using the `wheel` group:

```bash
sudo usermod -aG wheel newuser
```

The exact setup depends on the distribution's `sudo` configuration.

Grant administrative privileges only when necessary.

---

## 48. Group Changes and Sessions

After adding a user to a group, the current login session may not immediately reflect the change.

You can:

```text
Log out
```

and log back in.

Or start a new session.

Then check:

```bash
groups
```

---

## 49. User Management Practice

For a safe practice environment, create:

```text
student1
student2
developers
```

Commands:

```bash
sudo groupadd developers

sudo useradd -m student1
sudo useradd -m student2

sudo passwd student1
sudo passwd student2

sudo usermod -aG developers student1
sudo usermod -aG developers student2
```

Verify:

```bash
id student1
id student2
groups student1
groups student2
```

---

## 50. Shared Directory Practice

Create:

```bash
sudo mkdir /shared-project
```

Change group:

```bash
sudo chgrp developers /shared-project
```

Set group permissions:

```bash
sudo chmod 770 /shared-project
```

Check:

```bash
ls -ld /shared-project
```

The intended permission pattern is:

```text
drwxrwx---
```

Conceptually:

```text
Owner   → Full access
Group   → Full access
Others  → No access
```

---

## 51. Group Inheritance Challenge

For a shared development directory, you may want new files to inherit the directory's group.

Set SGID:

```bash
sudo chmod g+s /shared-project
```

Check:

```bash
ls -ld /shared-project
```

You may see:

```text
drwxrws---
```

The `s` indicates the SGID bit.

This is useful for shared project directories.

---

## 52. Important Security Rules

Never:

```text
Share passwords
Store passwords in Git
Give everyone sudo access
Use root unnecessarily
Modify /etc/passwd manually
Modify /etc/shadow manually
Run unknown commands with sudo
Use dangerous recursive commands blindly
```

Prefer:

```text
Least privilege
Strong authentication
Groups for shared access
Minimal sudo privileges
Secure file permissions
```

---

## 53. Practice Commands

Run these in your Linux environment:

```bash
whoami
id
groups
echo $USER
echo $HOME
echo $SHELL
```

Then inspect:

```bash
getent passwd
getent group
```

Check logged-in users:

```bash
who
w
```

---

## 54. Challenge

Create:

```text
developers
tester
developer1
```

Requirements:

```text
developer1 → member of developers
tester      → member of developers
```

Verify:

```bash
id developer1
id tester
groups developer1
groups tester
```

Then create:

```text
/shared-project
```

Set:

```text
Owner → root
Group → developers
Permissions → 770
```

Verify:

```bash
ls -ld /shared-project
```

---

## 55. Interview Questions

### Q1. What is a UID?

UID is the numeric identifier assigned to a Linux user.

### Q2. What is a GID?

GID is the numeric identifier assigned to a group.

### Q3. What is the root user?

Root is the privileged administrative account on Linux.

### Q4. What does `whoami` do?

It displays the current effective username.

### Q5. What does `id` do?

It displays UID, GID, and group membership information.

### Q6. What is `/etc/passwd`?

It contains basic account information for users.

### Q7. Where is password-related account information stored?

On typical Linux systems, password hashes and related information are stored in `/etc/shadow`, which is restricted.

### Q8. What is the purpose of groups?

Groups allow permissions and access to be managed for multiple users collectively.

### Q9. What does `usermod -aG` do?

It adds a user to supplementary groups while preserving their existing supplementary group memberships.

### Q10. What is the difference between `who` and `whoami`?

```text
who    → Shows logged-in users
whoami → Shows the current user
```

### Q11. What does `userdel` do?

It removes a user account.

### Q12. What does `passwd` do?

It can set or change a user's password and can also be used for certain password-account operations.

### Q13. Why should root access be limited?

Because unrestricted administrative privileges can cause severe system damage if misused or compromised.

---

# Summary

Linux user management revolves around:

```text
Users
Groups
UIDs
GIDs
Home directories
Shells
Permissions
Privileges
```

Important commands:

```bash
whoami
id
groups
who
w
last
useradd
adduser
usermod
userdel
passwd
groupadd
groupdel
chown
chgrp
sudo
```

Important files:

```text
/etc/passwd
/etc/shadow
/etc/group
```

The core relationship is:

```text
User
 ↓
Groups
 ↓
Permissions
 ↓
Resources
```

For real systems, follow the principle of:

```text
Least Privilege
```

Give users only the access they actually need.

Next topic: Linux Processes and Process Management
