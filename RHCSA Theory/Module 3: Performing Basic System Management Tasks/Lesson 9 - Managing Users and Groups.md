# Lesson 9: Managing Users and Groups
- 9.1 Understanding the Purpose of User Accounts
- 9.2 Setting User Properties
- 9.3 Creating and Managing Users
- 9.4 Defining User Default Settings
- 9.5 Limiting User Access
- 9.6 Managing Group Membership
- 9.7 Creating and Managin Groups
- 9.8 Setting Password Properties

## 9.2 Setting User Properties
- name
- password
- UID: a unique identifier for users
- GID: the ID of the primary group
- GECOS: additional non-mandatory information about the user
- Home Directory: the environment where users create personal files
- Shell: the program that will be started after successful authentication

## 9.3 Creating and Managing Users
- useradd: create user accounts
- usermod: modify user accounts
- uderdel: delete uder accounts
- passwd: set passwords

## 9.4 Defining User Default Settings
- Use `useradd -D` to specify default settings
- Settings in /etc/default/useradd apply to `sueradd` only
- Alternative, write default settings to /etc/login.defs
- Files in /etc/skel are created to the user home directory upon creation

## 9.5 Limiting User Access
- User accounts can be temporarily locked
    - `usermod -L anna` will lock anna
    - `usermod -U anna` will unlock anna
- User accounts can be set to expire also
    - `usermod -e 2032-01-01 bill` expires user account bill on 01-01-2023
- Set /sbin/nologin as the shell for users that are not intended to log in at all
    - `usermod -s /sbin/nologin myapp`
   - `usermod -aG profs anna` 

## 9.6 Managing Group Membership
- Each user must be a member of at least one group
- Primary Group Membership is managed through /etc/password
- The user primary group becomes group-owner if a user creates a file
- Additional (secondary) groups can be defined as well
- Secondary Group Membership is managed through /etc/groups
- Temporarily set primary group membership using `newgrp`
- Use `id` see which groups a user is a member of

## 9.7 Creating and Managing Groups
- Use `groupadd` to add groups
- `groupdel` and `groupmod` can be used to delete and modify groups
- Use `lid -g groupname` to list all users that are members of a specific group
- Use `cat /etc/group` to the list of existing group

## Password encryption
- Encrypted passwords are stored in /etc/shadow
- The encrypted string shows 3 pieces of information\
    - The hashing algorithm
    - The random salt
    - The encrypted hash of the user password

### Manage Password Settings
- Basic password requirements are set in /etc/login.defs
- For advanced password properties, Pluggable Authentication Modules (PAM) can be used
    - Look for the pam_faillock module
- To change password settings for current users, use `chage` or `passwd` as root

## Lesson 9 Lab: Managing Users and Groups
- Make sure that new users require a password with a maximal validity of 90 days
```bash
sudo vim /etc/logic.defs

# edit the below:
PASS_MAX_DAYS 90
```
- Ensure that while creating users, an empty file with the name newfile is created to their home directory

```bash
touch /etc/skel/newfile
```

- Create users anna, audrey, linda, and lisa

```bash
useradd {anna,audrey,linda,lisa}
```

- Set the passwords for anna and audrey ti `password`, disanle the passwords for linda and lisa

```bash
echo password | passwd --stdin anna
# disable password by adding the below to /etc/sudoers or visudo
pass -l linda
linda ALL=NOPASSWD : ALL
lisa ALL=NOPASSWD : ALL
```

- Create the groups profs and students, and make users anna and audrey members of profs, and linda and lisa members of students

```bash
usermod -aG profs anna
groupmod -U anna,audrey profs
```