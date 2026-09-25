
# Linux SSH and Remote Access

## 1. What Is SSH?

SSH stands for:

```text
Secure Shell
```

SSH is a protocol used to securely connect to and manage a remote computer over a network.

Basic model:

```text
Your Computer
      ↓
     SSH
      ↓
Remote Linux Server
```

---

## 2. Why Use SSH?

SSH is commonly used for:

```text
Remote server administration
Running commands remotely
Managing cloud servers
Transferring files
Managing applications
Troubleshooting servers
```

---

## 3. SSH Client and Server

SSH communication normally involves:

```text
SSH Client
    ↓
SSH Server
```

The client is the computer initiating the connection.

The server is the computer accepting SSH connections.

---

## 4. SSH Client

On many Linux systems, the SSH client is available as:

```bash
ssh
```

Check:

```bash
ssh -V
```

Example:

```text
OpenSSH_...
```

---

## 5. SSH Server

The SSH server is commonly provided by:

```text
OpenSSH Server
```

The server process is commonly associated with:

```text
sshd
```

Check whether it is running on a system using systemd:

```bash
systemctl status ssh
```

On some distributions, the service may be named:

```text
sshd
```

---

## 6. Basic SSH Syntax

The basic syntax is:

```bash
ssh username@hostname
```

Example:

```bash
ssh user@192.168.1.20
```

This means:

```text
Connect to
username: user
host: 192.168.1.20
```

---

## 7. SSH Using a Hostname

Instead of an IP address:

```bash
ssh user@server
```

The hostname must resolve to the correct machine.

For example:

```text
ssh admin@myserver
```

---

## 8. SSH Port

SSH commonly uses:

```text
Port 22
```

You can specify another port with:

```bash
ssh -p 2222 user@server
```

Here:

```text
-p 2222
```

means connect using port 2222.

---

## 9. First SSH Connection

When connecting to a server for the first time, SSH may display a host-key verification message.

You may see something similar to:

```text
The authenticity of host ... can't be established.
```

SSH is asking whether you trust the server's host key.

Verify the server identity before accepting it, especially on production systems.

---

## 10. SSH Host Keys

SSH servers have host keys.

They help the client identify the server.

Conceptually:

```text
Server
  ↓
Host Key
  ↓
Client remembers identity
```

This helps detect unexpected changes to a server's identity.

---

## 11. Known Hosts

SSH stores known server identities in:

```text
~/.ssh/known_hosts
```

You can inspect it with:

```bash
cat ~/.ssh/known_hosts
```

Do not edit it blindly.

---

## 12. SSH Password Authentication

A server may allow password-based authentication.

Example:

```bash
ssh user@server
```

You may then be prompted for the user's password.

For production servers, SSH keys are generally preferred over passwords.

---

## 13. SSH Key Authentication

SSH keys use a key pair:

```text
Private Key
Public Key
```

Conceptually:

```text
Your Computer
    │
    ├── Private Key
    │
    └── Public Key
              ↓
        Remote Server
```

The private key should remain secret.

---

## 14. Private Key

The private key must be protected.

Common key files include:

```text
~/.ssh/id_ed25519
```

or:

```text
~/.ssh/id_rsa
```

Never share your private SSH key.

---

## 15. Public Key

The corresponding public key commonly ends with:

```text
.pub
```

Example:

```text
~/.ssh/id_ed25519.pub
```

The public key can be placed on a remote server for authentication.

---

## 16. Generate an SSH Key

A modern choice is Ed25519.

Run:

```bash
ssh-keygen -t ed25519
```

You may be asked where to save the key.

The default location is commonly:

```text
~/.ssh/id_ed25519
```

---

## 17. Passphrase

When creating an SSH key, you can protect the private key with a passphrase.

This provides an additional layer of security.

Conceptually:

```text
Private Key
     ↓
Passphrase protection
     ↓
Safer key storage
```

Do not use an easily guessable passphrase.

---

## 18. SSH Key Files

After generating an Ed25519 key pair, you may have:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

Remember:

```text
id_ed25519
→ private key

id_ed25519.pub
→ public key
```

---

## 19. `ssh-copy-id`

On many Linux systems, you can copy your public key to a remote server with:

```bash
ssh-copy-id user@server
```

After successful setup, key-based authentication may allow you to connect without entering the account password each time.

---

## 20. Manual Public Key Installation

The public key is normally added to:

```text
~/.ssh/authorized_keys
```

on the remote user's account.

Conceptually:

```text
Your public key
      ↓
Remote server
      ↓
~/.ssh/authorized_keys
```

---

## 21. `authorized_keys`

The file:

```text
~/.ssh/authorized_keys
```

contains public keys allowed to authenticate as that user.

View it with:

```bash
cat ~/.ssh/authorized_keys
```

Only inspect or modify it when you understand the consequences.

---

## 22. SSH Directory Permissions

The SSH directory is commonly:

```text
~/.ssh
```

A typical secure permission is:

```text
700
```

You can set it with:

```bash
chmod 700 ~/.ssh
```

---

## 23. Private Key Permissions

A private key should normally not be readable by other users.

Example:

```bash
chmod 600 ~/.ssh/id_ed25519
```

This means the owner can read and write the key.

---

## 24. SSH Configuration

The client configuration file is commonly:

```text
~/.ssh/config
```

This lets you create convenient SSH aliases.

Example:

```text
Host myserver
    HostName 192.168.1.20
    User admin
    Port 22
```

Then connect using:

```bash
ssh myserver
```

---

## 25. SSH Config Benefits

Instead of repeatedly typing:

```bash
ssh admin@192.168.1.20
```

you can configure:

```text
Host myserver
    HostName 192.168.1.20
    User admin
```

Then:

```bash
ssh myserver
```

This is especially useful when managing multiple servers.

---

## 26. File Transfer With SCP

SCP means:

```text
Secure Copy Protocol
```

It can copy files over SSH.

Copy a local file to a remote server:

```bash
scp file.txt user@server:/home/user/
```

---

## 27. Copy From Remote Server

To copy a remote file to your current directory:

```bash
scp user@server:/home/user/file.txt .
```

The:

```text
.
```

means the current directory.

---

## 28. Copy a Directory

Use:

```bash
scp -r project/ user@server:/home/user/
```

The:

```text
-r
```

means recursively copy directories and their contents.

---

## 29. SFTP

SFTP means:

```text
SSH File Transfer Protocol
```

Start an SFTP session:

```bash
sftp user@server
```

You can then transfer files interactively.

---

## 30. Common SFTP Commands

Inside an SFTP session:

```text
ls
```

lists remote files.

```text
pwd
```

shows the remote working directory.

```text
lpwd
```

shows the local working directory.

```text
get file.txt
```

downloads a file.

```text
put file.txt
```

uploads a file.

```text
exit
```

closes the session.

---

## 31. Remote Command Execution

SSH can execute a command without opening an interactive shell.

Example:

```bash
ssh user@server "hostname"
```

You can run:

```bash
ssh user@server "uptime"
```

or:

```bash
ssh user@server "ls -la"
```

---

## 32. Remote System Information

For example:

```bash
ssh user@server "uname -a"
```

This executes `uname -a` on the remote system.

Your local terminal displays the remote command's output.

---

## 33. SSH Connection Test

Use:

```bash
ssh -v user@server
```

The `-v` option enables verbose output.

This can help diagnose:

```text
Authentication problems
Connection problems
Key problems
Configuration problems
```

---

## 34. More Verbose SSH Debugging

You can use:

```bash
ssh -vv user@server
```

or:

```bash
ssh -vvv user@server
```

More `v` characters produce more debugging information.

Use this when troubleshooting SSH connections.

---

## 35. Checking SSH Port

Before connecting, you can check whether port 22 is reachable.

One option is:

```bash
nc -zv server 22
```

if `nc` is installed.

Another option:

```bash
ssh -v user@server
```

SSH itself will provide useful connection information.

---

## 36. SSH Server Configuration

The SSH server configuration is commonly:

```text
/etc/ssh/sshd_config
```

This file can control settings such as:

```text
Port
Authentication methods
Password authentication
Root login
Allowed users
```

Do not change SSH server configuration without understanding the setting.

---

## 37. Editing `sshd_config`

Before changing the configuration, create a backup:

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```

Then edit carefully.

For example:

```bash
sudo nano /etc/ssh/sshd_config
```

---

## 38. Validate SSH Configuration

Before restarting the SSH service, validate the configuration:

```bash
sudo sshd -t
```

If the command returns no output, the configuration is generally syntactically valid.

Always verify before restarting a remotely accessed server.

---

## 39. Restarting SSH

On systems using systemd:

```bash
sudo systemctl restart ssh
```

Some distributions use:

```bash
sudo systemctl restart sshd
```

Check the service name used by your system.

---

## 40. Checking SSH Service

Use:

```bash
sudo systemctl status ssh
```

or:

```bash
sudo systemctl status sshd
```

You want to see that the service is running successfully.

---

## 41. Enable SSH at Boot

On systemd systems:

```bash
sudo systemctl enable ssh
```

This configures the SSH service to start automatically during boot.

---

## 42. Starting SSH

If the SSH server is not running:

```bash
sudo systemctl start ssh
```

Then check:

```bash
sudo systemctl status ssh
```

---

## 43. Root Login

Direct SSH login as root is generally discouraged.

A safer approach is:

```text
Normal user
    ↓
SSH
    ↓
sudo
    ↓
Administrative task
```

This provides better accountability and reduces the risk of direct root access.

---

## 44. `sudo` Over SSH

You can connect as a normal user:

```bash
ssh user@server
```

Then use:

```bash
sudo command
```

Example:

```bash
sudo apt update
```

---

## 45. SSH Security Basics

Important practices:

```text
Use strong authentication
Protect private keys
Prefer key-based authentication
Avoid unnecessary root login
Keep SSH software updated
Use firewall rules appropriately
Use trusted networks
Monitor login activity
```

---

## 46. Protecting Private Keys

Never send your private key through:

```text
Email
Chat
GitHub
Public websites
Untrusted cloud storage
```

Never commit:

```text
id_rsa
id_ed25519
```

to a Git repository.

---

## 47. `.gitignore` and SSH Keys

If a project contains sensitive local files, make sure they are excluded from Git.

For example:

```text
.ssh/
*.pem
*.key
```

However, do not blindly add broad patterns without understanding what they exclude.

---

## 48. SSH Agent

`ssh-agent` can temporarily hold private keys in memory.

Start it:

```bash
eval "$(ssh-agent -s)"
```

Add a key:

```bash
ssh-add ~/.ssh/id_ed25519
```

List loaded keys:

```bash
ssh-add -l
```

---

## 49. Why Use `ssh-agent`?

Without an agent, you may need to provide your key passphrase repeatedly.

With an agent:

```text
Private Key
     ↓
ssh-agent
     ↓
SSH connections
```

The agent can use the loaded key during the session.

---

## 50. Remote Administration Workflow

A basic workflow:

```text
Find server
     ↓
Check network connectivity
     ↓
Connect using SSH
     ↓
Authenticate
     ↓
Inspect system
     ↓
Perform task
     ↓
Exit safely
```

Example:

```bash
ssh user@server
hostname
uptime
df -h
exit
```

---

## 51. `exit`

To close an SSH session:

```bash
exit
```

or press:

```text
Ctrl + D
```

You return to your local shell.

---

## 52. Practical Example

Suppose you have a remote machine:

```text
IP: 192.168.1.20
User: admin
```

Connect:

```bash
ssh admin@192.168.1.20
```

Check hostname:

```bash
hostname
```

Check uptime:

```bash
uptime
```

Check disk:

```bash
df -h
```

Exit:

```bash
exit
```

---

## 53. Practical Example: SSH Key

Generate a key:

```bash
ssh-keygen -t ed25519
```

Copy the public key:

```bash
ssh-copy-id user@server
```

Connect:

```bash
ssh user@server
```

Verify that key-based authentication works.

---

## 54. Practical Example: SCP

Create a test file:

```bash
echo "SSH practice" > test.txt
```

Copy it:

```bash
scp test.txt user@server:/home/user/
```

Then connect:

```bash
ssh user@server
```

Check:

```bash
ls -l ~/test.txt
```

---

## 55. Practice

If you have access to another Linux machine or a safe test server:

### Step 1

Check SSH:

```bash
ssh -V
```

### Step 2

Check your SSH directory:

```bash
ls -la ~/.ssh
```

### Step 3

Check whether an SSH key exists:

```bash
ls ~/.ssh/*.pub
```

### Step 4

If you do not have a key, create one:

```bash
ssh-keygen -t ed25519
```

### Step 5

Connect to your test server:

```bash
ssh user@server
```

Replace:

```text
user
server
```

with your actual values.

---

## 56. Practice: Remote Commands

Run:

```bash
ssh user@server "hostname"
```

Then:

```bash
ssh user@server "uptime"
```

Then:

```bash
ssh user@server "df -h"
```

Observe that the commands execute on the remote machine.

---

## 57. Practice: File Transfer

Create:

```bash
echo "Linux SSH practice" > ssh-test.txt
```

Upload:

```bash
scp ssh-test.txt user@server:/tmp/
```

Connect:

```bash
ssh user@server
```

Verify:

```bash
cat /tmp/ssh-test.txt
```

Then remove the practice file:

```bash
rm /tmp/ssh-test.txt
```

---

## 58. Challenge

Create an SSH workflow where you can:

```text
1. Connect to a remote machine
2. Check its hostname
3. Check uptime
4. Check disk space
5. Check memory
6. Transfer a file
7. Execute a remote command
8. Exit
```

Useful commands:

```bash
ssh
scp
sftp
hostname
uptime
df -h
free -h
exit
```

---

## 59. Interview Questions

### Q1. What is SSH?

SSH is a secure protocol for remote login and command execution over a network.

### Q2. What port does SSH commonly use?

```text
22
```

### Q3. What is `sshd`?

The SSH server daemon that accepts SSH connections.

### Q4. What is the difference between an SSH private key and public key?

```text
Private key → kept secret
Public key  → placed on the remote server
```

### Q5. Where is the public key commonly stored on the server?

```text
~/.ssh/authorized_keys
```

### Q6. What is `ssh-copy-id`?

A utility commonly used to install a user's public SSH key on a remote account.

### Q7. What does `scp` do?

It securely copies files over SSH.

### Q8. What is SFTP?

SSH File Transfer Protocol, used for secure file transfer over SSH.

### Q9. What does `ssh -v` do?

It provides verbose connection and authentication information for troubleshooting.

### Q10. What is `~/.ssh/config`?

A per-user SSH client configuration file.

### Q11. What is `/etc/ssh/sshd_config`?

The commonly used SSH server configuration file.

### Q12. Why should private SSH keys never be shared?

Anyone who obtains an usable private key may be able to authenticate as the associated identity, depending on the server configuration and key protections.

### Q13. Why use `ssh-agent`?

It can securely hold loaded private keys for use during SSH authentication.

### Q14. Why is direct root SSH login generally discouraged?

Using a normal account with `sudo` provides better control and reduces direct exposure of the root account.

### Q15. What command validates the SSH server configuration?

```bash
sudo sshd -t
```

---

# Summary

Important SSH concepts:

```text
SSH
SSH Client
SSH Server
sshd
Host Key
Private Key
Public Key
authorized_keys
SSH Agent
SCP
SFTP
Remote Commands
```

Important commands:

```bash
ssh user@server
ssh -p PORT user@server
ssh -v user@server
ssh-keygen -t ed25519
ssh-copy-id user@server
scp file user@server:/path/
sftp user@server
ssh-add
ssh-add -l
ssh-agent
```

Important files:

```text
~/.ssh/
~/.ssh/config
~/.ssh/known_hosts
~/.ssh/authorized_keys
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
/etc/ssh/sshd_config
```

Basic SSH flow:

```text
Client
  ↓
Server
  ↓
Host verification
  ↓
Authentication
  ↓
Remote shell
  ↓
Commands
  ↓
exit
```

The key rule:

```text
Never expose or commit your SSH private key.
```

Next topic: Linux Users, Groups and Permissions

Commit message:

```text
Add Linux SSH and remote access guide
```
