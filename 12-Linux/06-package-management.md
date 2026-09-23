
# Linux Package Management

## 1. What Is Package Management?

Package management is the process of:

```text
Installing software
Updating software
Removing software
Searching for software
Managing dependencies
```

Linux distributions use package managers to make software installation easier.

---

## 2. What Is a Package?

A package is a collection of files required to install software.

A package may contain:

```text
Program files
Configuration files
Documentation
Dependencies information
Metadata
```

Example:

```text
Package
   ↓
Install
   ↓
Software available on system
```

---

## 3. Package Manager

A package manager is a tool that manages software packages.

Different Linux distributions use different package managers.

Common examples:

```text
Debian / Ubuntu → APT
Fedora / RHEL   → DNF
Arch Linux      → Pacman
openSUSE        → Zypper
```

---

## 4. APT

APT stands for:

```text
Advanced Package Tool
```

It is commonly used on:

```text
Ubuntu
Debian
Linux Mint
```

For this lesson, the examples will mainly use APT.

---

## 5. Check Linux Distribution

Before using package-management commands, identify your distribution.

Run:

```bash
cat /etc/os-release
```

You may see:

```text
NAME="Ubuntu"
VERSION="..."
ID=ubuntu
```

---

## 6. Check Package Manager

If your system is Debian or Ubuntu based, check:

```bash
apt --version
```

Example:

```text
apt 2.x.x
```

---

## 7. Updating Package Information

Run:

```bash
sudo apt update
```

This updates the local package information from configured repositories.

Important:

```text
apt update
```

does not normally upgrade all installed software.

It refreshes package information.

---

## 8. Upgrading Packages

Run:

```bash
sudo apt upgrade
```

This upgrades installed packages when updates are available.

Typical workflow:

```bash
sudo apt update
sudo apt upgrade
```

---

## 9. Why `apt update` Comes First

Think of it like:

```text
Repository information
        ↓
   apt update
        ↓
Latest package information
        ↓
   apt upgrade
        ↓
Install available updates
```

Running `upgrade` without recently refreshing package information may mean you are working with older package metadata.

---

## 10. Installing Software

Use:

```bash
sudo apt install package-name
```

Example:

```bash
sudo apt install git
```

APT resolves required dependencies and installs the package.

---

## 11. Installing Multiple Packages

You can install multiple packages in one command:

```bash
sudo apt install git curl wget
```

APT will process the requested packages and their dependencies.

---

## 12. Removing Software

Use:

```bash
sudo apt remove package-name
```

Example:

```bash
sudo apt remove git
```

This removes the package while generally preserving configuration files.

---

## 13. Purging Software

Use:

```bash
sudo apt purge package-name
```

Example:

```bash
sudo apt purge git
```

Purging removes the package and its associated system configuration files where applicable.

---

## 14. Remove Unused Dependencies

Use:

```bash
sudo apt autoremove
```

This can remove packages that were automatically installed as dependencies and are no longer required.

Review the proposed changes before confirming.

---

## 15. Searching for Packages

Use:

```bash
apt search package-name
```

Example:

```bash
apt search python3
```

This searches package information available through configured repositories.

---

## 16. Showing Package Information

Use:

```bash
apt show package-name
```

Example:

```bash
apt show git
```

This can show:

```text
Package name
Version
Architecture
Dependencies
Description
Repository information
```

---

## 17. Checking Whether a Package Is Installed

One option is:

```bash
dpkg -l package-name
```

Example:

```bash
dpkg -l git
```

Another option is:

```bash
apt list --installed
```

---

## 18. Listing Installed Packages

Run:

```bash
apt list --installed
```

This may produce a very long list.

You can search it with:

```bash
apt list --installed | grep git
```

---

## 19. `dpkg`

APT is a higher-level package-management tool.

Under Debian-based systems, the lower-level package tool is:

```text
dpkg
```

APT handles repositories and dependency resolution.

DPKG directly manages Debian package files.

---

## 20. `.deb` Packages

Debian-based distributions commonly use:

```text
.deb
```

package files.

Example:

```text
software_1.0_amd64.deb
```

---

## 21. Installing a `.deb` Package

Modern APT can install a local `.deb` file:

```bash
sudo apt install ./package.deb
```

The `./` tells APT that the package is a local file.

---

## 22. Installing With `dpkg`

You can also install a `.deb` directly:

```bash
sudo dpkg -i package.deb
```

However, `dpkg` itself does not resolve dependencies in the same way APT does.

If dependency problems occur, APT can often help resolve them.

---

## 23. Fixing Broken Dependencies

A commonly used command is:

```bash
sudo apt --fix-broken install
```

This asks APT to attempt to resolve broken dependencies.

Do not run repair commands blindly. Read what packages APT proposes to change.

---

## 24. Package Repositories

A repository is a location containing packages and package metadata.

Conceptually:

```text
Your Computer
      ↓
Package Manager
      ↓
Repository
      ↓
Package
      ↓
Install
```

Repositories may be official or third-party.

---

## 25. Why Repositories Matter

Repositories provide:

```text
Software packages
Package versions
Security updates
Dependency information
Package metadata
```

Using trusted repositories reduces the risk of installing malicious or modified software.

---

## 26. Security Updates

Regular updates can provide:

```text
Bug fixes
Security fixes
Performance improvements
New features
```

A common maintenance routine is:

```bash
sudo apt update
sudo apt upgrade
```

---

## 27. Checking Upgradable Packages

Run:

```bash
apt list --upgradable
```

This shows packages for which newer versions are available according to the current package information.

---

## 28. Upgrade vs Full Upgrade

APT provides:

```bash
sudo apt upgrade
```

and:

```bash
sudo apt full-upgrade
```

`upgrade` is generally more conservative.

`full-upgrade` can make additional package changes, including removing packages if necessary to complete an upgrade.

Use `full-upgrade` carefully.

---

## 29. Package Cache

Downloaded package files may be stored in the APT cache.

You can inspect the cache with:

```bash
ls /var/cache/apt/archives/
```

---

## 30. Cleaning Package Cache

APT provides:

```bash
sudo apt clean
```

This removes downloaded package files from the local cache.

Another command is:

```bash
sudo apt autoclean
```

which removes package files that can no longer be downloaded from repositories.

---

## 31. Checking Package Installation Files

With `dpkg`, you can see which files belong to an installed package:

```bash
dpkg -L package-name
```

Example:

```bash
dpkg -L git
```

---

## 32. Finding Which Package Owns a File

For installed packages, you can use:

```bash
dpkg -S /path/to/file
```

Example:

```bash
dpkg -S /usr/bin/git
```

This can identify the package that installed the file.

---

## 33. Package Version

Check a package version with:

```bash
apt policy package-name
```

Example:

```bash
apt policy git
```

This can show:

```text
Installed version
Candidate version
Repository versions
```

---

## 34. Installing a Specific Version

APT may allow installation of a specific available version:

```bash
sudo apt install package-name=version
```

Example:

```bash
sudo apt install package-name=1.2.3
```

The exact version must be available from your configured repositories.

Do not blindly copy version numbers from another system.

---

## 35. Holding a Package

APT can hold a package so it is not automatically upgraded.

Example:

```bash
sudo apt-mark hold package-name
```

Check held packages:

```bash
apt-mark showhold
```

Remove the hold:

```bash
sudo apt-mark unhold package-name
```

Use package holds carefully because they can prevent important security updates.

---

## 36. Package Dependencies

Software often depends on other packages.

Example:

```text
Application
    ↓
Library A
    ↓
Library B
```

APT can resolve many dependencies automatically.

This is one of the major advantages of using a package manager.

---

## 37. Dependency Example

Suppose:

```text
Program A
```

requires:

```text
Library B
Library C
```

When you install Program A:

```text
sudo apt install program-a
```

APT may automatically install:

```text
Program A
Library B
Library C
```

---

## 38. Why Dependencies Matter

Without dependency management, you might have to manually find:

```text
Libraries
Correct versions
Required packages
Compatibility requirements
```

Package managers automate much of this work.

---

## 39. Package Configuration

Some packages require configuration during installation.

For example:

```text
Web server
Database server
Network service
```

The package manager may ask configuration questions during installation.

Read the prompts before accepting defaults.

---

## 40. Reinstalling a Package

If a package installation becomes corrupted, you may reinstall it:

```bash
sudo apt install --reinstall package-name
```

Example:

```bash
sudo apt install --reinstall curl
```

Use this only when there is a reason to reinstall.

---

## 41. Downloading Without Installing

APT can download a package without installing it:

```bash
apt download package-name
```

Example:

```bash
apt download curl
```

The downloaded package will normally appear in the current directory.

---

## 42. Package History

On Debian-based systems, package activity can often be investigated through:

```text
/var/log/apt/
```

For example:

```bash
ls /var/log/apt/
```

You may find logs related to package operations.

---

## 43. APT Log

A common log is:

```text
/var/log/apt/history.log
```

View it with:

```bash
less /var/log/apt/history.log
```

This can help identify packages that were installed, upgraded, or removed.

---

## 44. `less`

The `less` command allows you to view long text files page by page.

Example:

```bash
less /var/log/apt/history.log
```

Useful keys:

```text
Space → next page
b     → previous page
q     → quit
```

---

## 45. Package Management Safety

Before installing software, ask:

```text
Is the source trusted?
Do I actually need this package?
What dependencies will be installed?
Will existing packages be changed?
```

For important systems, review the proposed changes carefully.

---

## 46. Avoid Random Installation Commands

Do not blindly copy commands such as:

```bash
curl ... | sudo bash
```

from unknown websites.

This can execute downloaded content with administrative privileges.

Prefer:

```text
Official repositories
Official documentation
Trusted package sources
Verified software releases
```

---

## 47. Basic Package Management Workflow

A normal workflow:

```text
Identify software
      ↓
Search package
      ↓
Check package information
      ↓
Install
      ↓
Verify
      ↓
Update regularly
      ↓
Remove when no longer needed
```

Commands:

```bash
apt search package
apt show package
sudo apt install package
apt list --installed
sudo apt remove package
```

---

## 48. Practice

First identify your distribution:

```bash
cat /etc/os-release
```

Check APT:

```bash
apt --version
```

Update package information:

```bash
sudo apt update
```

Search for Git:

```bash
apt search git
```

Show information:

```bash
apt show git
```

Check whether Git is installed:

```bash
dpkg -l git
```

---

## 49. Practice Installing a Package

If Git is not installed:

```bash
sudo apt install git
```

Verify:

```bash
git --version
```

Example:

```text
git version 2.x.x
```

The exact version depends on your system.

---

## 50. Practice Checking Updates

Run:

```bash
apt list --upgradable
```

Then:

```bash
sudo apt upgrade
```

Review the packages and changes before confirming.

---

## 51. Practice Package Information

Run:

```bash
apt show curl
```

Find:

```text
Package
Version
Architecture
Dependencies
Description
```

Then:

```bash
apt policy curl
```

Compare the installed and candidate versions if available.

---

## 52. Practice Removing a Package

Choose a package that you installed specifically for practice and no longer need.

For example:

```bash
sudo apt remove package-name
```

Then verify:

```bash
dpkg -l package-name
```

Do not remove important system packages just for practice.

---

## 53. Challenge

Complete this workflow for `curl`:

```text
1. Search for curl
2. Show curl information
3. Check whether curl is installed
4. Check its version
5. Check its package policy
```

Commands:

```bash
apt search curl
apt show curl
dpkg -l curl
curl --version
apt policy curl
```

---

## 54. Challenge: Package Investigation

Pick one installed package.

Run:

```bash
apt show PACKAGE
```

Then:

```bash
dpkg -L PACKAGE
```

Then:

```bash
apt policy PACKAGE
```

Find:

```text
Package name
Installed version
Candidate version
Dependencies
Installed files
Description
```

Replace:

```text
PACKAGE
```

with the actual package name.

---

## 55. Interview Questions

### Q1. What is a package?

A package is a collection of software files and metadata used to install and manage software.

### Q2. What is a package manager?

A tool that installs, updates, removes, and manages software packages and dependencies.

### Q3. What is APT?

APT is the package-management system commonly used by Debian-based Linux distributions.

### Q4. What does `apt update` do?

It refreshes package information from configured repositories.

### Q5. What does `apt upgrade` do?

It upgrades installed packages when newer versions are available.

### Q6. What is the difference between `apt remove` and `apt purge`?

```text
remove → removes the package
purge  → removes the package and associated configuration files where applicable
```

### Q7. What does `apt autoremove` do?

It can remove automatically installed dependencies that are no longer required.

### Q8. What is a repository?

A source containing software packages and package metadata.

### Q9. What is a dependency?

A package or library required by another piece of software.

### Q10. What is `dpkg`?

A lower-level Debian package-management tool used to install and manage `.deb` packages.

### Q11. What is a `.deb` file?

A package file format commonly used by Debian-based Linux distributions.

### Q12. How do you install a package?

```bash
sudo apt install package-name
```

### Q13. How do you search for a package?

```bash
apt search package-name
```

### Q14. How do you check installed packages?

```bash
apt list --installed
```

### Q15. How do you check available updates?

```bash
apt list --upgradable
```

---

# Summary

Package management allows you to:

```text
Search software
Install software
Update software
Remove software
Manage dependencies
Inspect package information
```

Important APT commands:

```bash
sudo apt update
sudo apt upgrade
sudo apt install PACKAGE
sudo apt remove PACKAGE
sudo apt purge PACKAGE
sudo apt autoremove
apt search PACKAGE
apt show PACKAGE
apt policy PACKAGE
apt list --installed
apt list --upgradable
```

Important DPKG commands:

```bash
dpkg -l
dpkg -L PACKAGE
dpkg -S FILE
dpkg -i PACKAGE.deb
```

Important concept:

```text
Repository
    ↓
Package Manager
    ↓
Package
    ↓
Dependencies
    ↓
Installed Software
```

The key rule:

```text
Use trusted package sources and review important changes before confirming.
```

Next topic: Linux Services and systemd

Commit message:

```text
Add Linux package management guide
```
```
