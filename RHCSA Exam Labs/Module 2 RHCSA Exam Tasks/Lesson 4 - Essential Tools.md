## 4.1 Configuring Remote Repository Access
### Task: Confuguring remote repository access
- Configure your system such that it can use the repository `https://repository.example.com`
- Ensure that no GPG checks will be done while accessing this repository
- Ensure that the client will not actually use this directory
- To verify your work, the repository should not show while using the `dnf repolist` command, but its configuration should exist

### Solution
```bash
dnf config-manager --add-repo=https://repository.example.com

vim /etc/yum.repos.d/repository.example.com.repo
# start editing repository.example.com
[repository.example.com]
name=created by dnf config-manager from https://repository.example.com
baseurl=https://repository.example.com
enabled=0
gpgcheck=1
# end editing

dnf repolist # to verify the work
```

## 4.2 Configuring local repository access
### Task: Configure local repository access
- Make an ISO file of your installation disk and store it as `/rhel9.iso`
- Mount it persistently on the directory `/repo` on your local server.
- Configure your local server to access this mounted disk as a repository.
- Verify that you can install packages from this repository.

## 4.3 Managing permissions
### Task: Managing Permissions
- Create a directory with the name `/data/profs`
- Create a group `groups`
- Create a user `linda`
- Configure permissions such that user `linda` can NOT read or write files in the directory `/data/profs`, but is allowed to change permissions on the directory `/data/profs`
- Members of the group `profs` should be able to read and write files in the directory `/data/profs`
- Nobody else should have access to the directory
    - Solution:

    ```bash
    # under root privileges
    groupadd profs
    useradd linda -G profs
    mkdir -p /data/profs
    chown linda:profs /data/profs
    chmod 070 /data/profs
    ```

### Key Elements:
- Basic permissions are based on ownership
- Each file has a user-owner, a group-owner, and the other entities
- While evaluating permissions, Linux checks user-ownership and group-ownership. If the user accessing a fil eis neither user-owner, not group-owner, permissions for others are assigned.
- The check will exit on match: if a user is user-owner, group permissions and perissions for other are not checked.

## 4.4 Finding files
### Task
- Find all files with a size bigger than 100 MiB, and write a long listing of these files to the file `/tmp/files`
- Solution:

    ```bash
    find / -size +100M -type f -exec ls -l {} \; 2>/dev/null > /tmp/myfiles
    ```
### Key Elements
- `find` is used to find files based on any property
- To perform a command on the result of the `find` command, add `-exec command {} \;` to `find` command
- `grep` is used to search for a regular expression in files or command output
- Different sets of regular expressions exist, including based regular expressions and extended regular expressions.
- To use extended regular expressions, use `grep -e`
- `awk` can be used as a powerfull alternative to `grep`