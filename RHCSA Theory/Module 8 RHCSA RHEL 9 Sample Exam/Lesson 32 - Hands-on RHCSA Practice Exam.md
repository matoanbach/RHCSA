# Lesson 32.1 Exploring RHCSA Practice Exam Assignments
### Setting up a Base Server
- Install two servers using the minimal installation pattern. Use the names server1.example.com and server2.example.com and use DHCP to get an IP address from the local DNS server.
- Solution:
    - On both servers, config both using the below configuration:
    - `/`: 15GiB
    - `/home`: 3 GiB
    - `/boot`: 500 MiB
    - `swap`: 1 GiB
    - `/boot/efi`: 500 MiB

- Ensure that the partition for `/` is 15GiB. Also create a 1GiB swap partition. Do NOT register the servers with Red Hat

### Resetting the Root Password
- Use the appropriate solution to reset the root password on server2, assuming that you have lost the root password and you have no administrator access to the server anymore.

- Solution:
    - Reboot and then press `e`. Add line `init=/bin/bash`
    - `mount -o remount,rw /`
    - `passwd root`
    - `touch /.autorelabel`
    - `exec /usr/lib/systemd/systemd`

### Configuring a Repository
- On both servers, create an ISO file based on the installation DVD. Mount this ISO file persistently and configure the servers to use the local ISO file as the repositories. After carefully completing this assignment, you should be able to install software on both servers.

- Solution:
    - Install the ISO file to DVD on both RHEL instances
      - On **VMWare**, go VM > Settings > Add a CD/DVD > Pick an RHEL image on your host machine filesystem.
    - `dd if=/dev/sr0 of=/rhel9.iso bs=1M`
    - `mkdir /repo`
    - `cp /etc/fstab /etc/fstab.bak`
    - `echo "/rhel9.iso     /repo    iso9660        defaults  0  0" >> /etc/fstab`
    - `dnf config-manager --add-repo file:///repo/BaseOS`
    - `dnf config-manager --add-repo file:///repo/AppStream`

### Managing Partitions
- On server1, use your virtualization software to increase the size of your primary disk in such a way that at least 10 GiB of unallocated disk space is available.
    - Solution:
        - Shutdown the RHEL instance, and add one more Disk using `Parallels` control center
- In the free disk space, create a 1 GiB partition and format it with vfat filesystem. Make sure it is mounted persistently on the `/winfile` directory.

- Also create a 1 GiB swap partition and ensure it is mounted persistently.

- Solution:
    ```bash
    # note UUID is optional
    fdisk /dev/sdb
    p # to create a new partition
    t # to change the partition type to fat32 (vfat) partition

    p # to create a new partition
    t # to change the partition type to swap 

    w # write and exit fdisk
    mkfs.vfat /dev/sdb1
    swapon /dev/sdb1
    lsblk --output=UUID /dev/sdb1 | awk '{print $2}' >> /etc/fstab
    # add the line below to /etc/fstab 
    UUID=..... /winfile vfat defaults 0 0

    mkswap /dev/sdb2
    swapon /dev/sdb2
    lsblk --output=UUID /dev/sdb2 | awk '{print $2}' >> /etc/fstab
    # add the line below to /etc/fstab 
    UUID=..... none swap defaults 0 0

    findmnt --verify # to verify everything before reboot
    ```

### Managing LVM Logical Volumes
- Create a logical volume with the name myfiles. Ensure it uses 8 MiB extents. Configure the volume to use 75 extents. Format it with the ext4 file system and ensure it mounts persistently on `/myfiles`
- If volume groups need to be created, create them as needed
    - Solution:
        - `fdisk /dev/sdb`
        - Then create an extended partion and then create logical partitions with LVM type
        - `pvcreate /dev/sdb1`
        - `vgcreate -s 8M myvg /dev/sdb1`
        - `lvcreate -n myfiles -l 75 myvg`
        - `mkfs.ext4 /dev/myvg/myfiles`
- Increase the size of the `/` logical volume by 5 GiB
    - Solution:
        - `lvextend -L +5G /dev/myvs/myfiles`
     

### Creating Users and Groups
- Create a user lisa. Ensure she has the password set to "password" and is using UID 1234. She must be a member of the secondary group sales
    - Solution:
        - `useradd lisa`
        - `passwd lisa` and then enter `password` as password
        - `usermod --uid 1234 lisa`
- Create a user myapp. Ensure this user cannot open an interactive shell.
    - Solution:
        - `useradd myapp`
        - `usermod -s /sbin/nologin myapp`
        - `usermod -s /bin/bash myapp`

### Managing Permissions
- Create a shared group directory `/sales`  and ensure that `lisa` is the owner of that directory. The owner and the group sales should have permissions to access this directory and read and write files in it. Other user should have no permission at all. Ensure that any new file that is created in this directory is group-owned by the group sales automatically
    - Solution:
        - `mkdir /sales`
        - `groupadd sales`
        - `usermod -aG sales lisa`
        - `chown lisa:sales /sales`
        - `chmod g+ws /sales` to set SGID so that any files or sub directories will inherit the rules from the parent folder.
        - note: SGID only works with group
### Scheduling Jobs
- Schedule a job that writes "hello folks" to syslog every Monday through Friday at 2 AM. Make sure this job is executed as the user lisa.
    - Solution:
        - `crontab -u lisa -e`
        ```bash
        SHELL=/bin/bash
        PATH=/sbin/:bin/:/usr/sbin:/usr/bin
        MAILTO=root

        0 2 * * * 1-5 logger "hello folks"
        ```

### Managing Containers as Services
- Create a container with the name mydb that runs the mariadb database as user lisa. The container should automatically be started at system start, regardless of whether or not user lisa is logging in. The container further should meet the following requirements:
    1. The host directory `/home/lisa/mydb` is mounted on the container directory `/var/lib/mysql`
    2. The container is accessible on host port 3206
    3. You do not have to create any databases in it.

    - Solution:
        - `su - lisa`
        - `podman login registry.access.redhat.com`
        - `podman unshare cat /proc/self/uid_map`
        - `podman unshare chown 1234:1234 /home/lisa/mydb`
        - `podman run -d -n mydb -e MYSQL_ROOT_PASSWORD=password -p 3206:3206 -v /home/lisa/mydb:/var/lib/mysql docker.io/alpine/mariadb`
        - `firewall-cmd --add-port=3206/tcp --permanent`
        - `firewall-cmd --reload`
        - `loginctl enable-linger lisa`
        - `mkdir -p ~/.config/systemd/user; cd ~/.conf/systemd/user`
        - `podman generate systemd --name mydb --files --new`
        - `systemctl --user daemon-reload`
        - `systemctl --user start container-mydb.service`
        - `systemctl --user status container-mydb.service`

### Managing Automount
- On server2, create the directories `/homes/user1` and `/homes/user2`. Use NFS to share these directories and ensure the firewall does not block access to these directories.
    - Solution:
        - `dnf install -y nfs-utils`
        - `systemctl enable --now nfs-server`
        - `firewall-cmd --add-service=mountd --permanent`
        - `firewall-cmd --add-service=rpc-bind --permanent`
        - `firewall-cmd --add-service=nfs --permanent`
        - `firewall-cmd --reload`
        - `vim /etc/exports`
            ```vi
            /homes/user1    *(rw,no_root_squash)
            /homes/user2    *(rw,no_root_squash)
            ```
        - `showmount -e server2`
- On server1, create a solution that automatically, on-demand mounts `server2:/homes/user1` on `/homes/user1` and also that automatically, on-demand mounts `server2:/homes/user2` on `/homes/user2` when these directories are accessed.
    - Solution:
        - `dnf install -y autofs`
        - `vim /etc/auto.master`
            ```vim
            /homes       /etc/auto.homes
            ```

        - `vim /etc/auto.homes`
            ```vim
            *   -rw     server2:/homes/& 
            ```


### Setting Time
- Configure server1 and server2 as an NTP client for pool.ntp.org.

### Managing SELinux
- Ensure that the Apache web server is installed and configure it to offer access on port 82.
- Copy the file `/etc/hosts` to `/tmp/hosts`. Next, move `/tmp/hosts` to the directory `/var/www/html/hosts` and ensure this file can be access by the Apache web server.
- `semanage fcontext`