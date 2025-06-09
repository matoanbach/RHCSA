# Lesson 8: Using root Privileges

- 8.1 Understanding the root User
- 8.2 Switching User with `su`
- 8.3 Performing Administrator Tasks with `sudo`
- 8.4 Managing `sudo` Configuration
- 8.5 Using `ssh` to Log In remotely

## 8.2 Switching User with `su`
- the su command is used to switch current user account from a shell environment
- if su is used with the `-` option, the complet environment of the target user is loaded
- While using su, the password of the target user is entered
- If the root user has a passowrd, `su - ` can be used
- Using `su -` to open a root shell is considered bad practice, use `sudo -i` instead
- The `su` command can be usefull for testing other user accounts

## 8.3 Performing Administrator Tasks with sudo
- `sudo` is more sucure mechanism to perform administration tasks
- Behind `sudo` is the /etc/sudoers configuration file
- While edit /etc/sudoers through visudo, very detailed administration privileges can be assigned
- To run an administration task using `sudo`, use `sudo command`
- This will prompt for the current user password, and run the command if this allowed through /etc/sudoers
- To open a root shell, `sudo -i` can be used

## 8.4 Managin sudo Configuration
- Sudo configuration is managed through /etc/sudoers
- Don't edit this file directly, only edit it through `visudo`
- Instead of editing /etc/sudoers, consider creating drop-in files in /etc/sudoers.d
- /etc/sudoers is installed from packages and may be overwritten, drop-in files will never be overwritten

### Providing Administrator Access
- Users that are a member of the group `wheel` get full sudo access
    - This is accomplished by `%wheel ALL=(ALL) ALL` in /etc/sudoers
    - User `usermod -aG wheel myuser` to add a user to the group wheel
- DO NOT enable the line `%wheel ALL=(ALL) NOPASSWORD: ALL`
    - It will provide full sudo access without entering a password and is very dangerous
- If you don't like entering your user password every five minutes, increase authentication token expiration by adding the folliwng
    - `Defaults timestamp_type=global,timestamp_timeout=60`

### Providing Access to Specific Tasks
- Use drop-in files to provide admin access to specific tasks
    - `lisa ALL=/sbin/useradd, /usr/bin/passwd`
- Consider using command arguments to make the commands more specific
    - `%users ALL=/bin/mount /dev/sdb, /bin/umount /dev/sdb`
    - `linda ALL=/usr/bin/passwd, !/usr/bin/passwd root`

### Using ssh to Log in Remotely 

- By default, all RHEL servers run a Secure Shell (SSH) server
- Use `systemctl` to verify
    - `systemctl status sshd`
- SSH access is allowed through the firewall by default
- Notice that root access is often denied
- Use `ssh` to connect to a remote server
    - On the remote server, use `ip a` to find the IP address
    - `ssh 192.168.29.100` will connect to this IP address using your current user account
    - `ssh user@192.168.100` will connect as a specific user

## Lesson 8 Lab: configuring `sudo`
- Use `useradd linda` to create a user linda
- Create a sudo configuration that allows linda to perform common user management tasks
    - Allow using `useradd`, `usermod` and `userdel`
    - Allow changing passwords, but not the password for user root
- Ensure that the user only needs to enter a password for `sudo` operations every 60 minutes