# Lesson 10: Scuring Files with Permissions

- 10.1 Understanding Ownership
- 10.2 Chaning File Ownership
- 10.3 Understanding Basic Permissions
- 10.4 Managing Basic Permission
- 10.5 Configuring Shared Group Directories
- 10.6 Applying Defailt Permissions

## 10.1 Understanding Ownership

- To determine which permissions a user has, Linux uses ownership
- Every file has a user-owner, a group-owner and the other entity that is also granted permissions (`ugo`)
- Linux permissions are not additive, if you're the owner, permissions are applied and that's all
- Use `ls -l` to display current ownership and associated permissions
- Best practice: Set ownership before modifying permission

## 10.2 Chaning File Ownership

- use `chown user[:group] file` to set user-ownership
- use `chgrp group file` to set group-ownership

## 10.3 Understanding Basic Permissions

|             | files  | dir           |
| ----------- | ------ | ------------- |
| read (4)    | open   | list          |
| write (2)   | modify | create/delete |
| execute (1) | run    | cd            |

- when `x` is appkied recursively, it would make directories as well as files executable
- in recurseive command, use `X` instead
    - Directories will be granted the execute permission
    - Files will only get the execute permission if it is set already elsewhere on the file

## 10.4 Managing Basic Permissions
- `chmod` is used to manage permissions
- It can be used to absolute or relative mode
- `chmod 750 myfile`
- `chmod +x myscript`

## 10.5 Configuring Shared Group Directories
- `chmod g+s mydir` will apply SGID to the directory
- `chmod +t mydir` assigns sticky bit to the directory
- In absolute mode, a four digit number is used, of which the first digit for the special permissions
- `chmod 3660 mydir` assigns SGID and sticky it, as well as rwx for user and group

## 10.6 Applying Default Permissions
- The `umask` is a shell setting that substracts the umask from the default permission
    - Default is set in /etc/bashrc
    - Set user-specific overrides in ~/.bashrc
- Default permissions for file are 666
- Default permissions for directory are 777

## Lesson 10 Lab: Managing Permissions
- Create a shread group directory structure /data/profs and /data/students that meets the following conditions
    - Members of the groups have full read and write access to their directories, others has no permissions at all

    ```bash
    mkdir /data/{profs,students}
    chgrp profs /data/profs 
    chgrp students /data/students
    chmod 770 /data/{profs,students} 
    ```

- Modify default permission settings such that normal users have a umask that allows the user and group to write, create and execute files and directories while denying all other access to others

```bash
umask 007
```