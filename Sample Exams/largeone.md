## Large Sample Exam
- Link: https://docs.google.com/document/d/1y_LHzl0LkQkBfgvlCLlfLjJntMfrNOXmPIwrh40tT9k/edit?pli=1&tab=t.1 

## Setup for Large Sample Exam:
- Build 2 virtual machines with RHEL 9.3 Server with GUI. Use a 20GB disk for the OS with default partitioning. Add an additional 20GB disk and a network interface. Do not configure the network interface or create a normal user account during installation. When you set up the network connections in a later task make sure they have an internet connection for the purpose of this exam, the actual exam will have no internet connection. You will need to be able to download images from Docker or the RH registry for the steps below.


## Tasks
1. Assume the root user password is lost, and your system is running in multi-user target with no current root session open. Reboot the system into an appropriate target level and reset the root user password to root1234. After completing this task, log in as the root user and perform the remaining tasks presented below.

2. On VM1 configure a network connection on the primary network device with IP address 192.168.0.241/24, gateway 192.168.0.1, and nameserver 192.168.0.1. Use different IP assignments based on your lab setup.
- Tips:
    - `ip route` to check the default gateway
- How to verify:
    - `ping [new ip]` to make the new IP is working
    - `ping 8.8.8.8` to make sure the gateway is working

3. On VM2 configure a network connection on the primary network device with IP address 192.168.0.242/24, gateway 192.168.0.1, and nameserver 192.168.0.1. Use different IP assignments based on your lab setup.

- Solution:
    ```bash
    nmcli con add con-name concon ifname ens160 ipv4.addresses 192.168.182.242/24 ipv4.gateway 192.168.182.2 ipv4.method manual type ethernet
    ```

4. On VM1 set the system hostname to rhcsa1.example.com and alias rhcsa1. Make sure that the new hostname is reflected in the command prompt.

5. On VM2 set the system hostname to rhcsa2.example.com and alias rhcsa2. Make sure that the new hostname is reflected in the command prompt.

6. Run “ping -c2 rhcsa2” on rhcsa1. Run “ping -c2 rhcsa1” on rhcsa2. You should see 0% loss in both outputs.

7. On rhcsa1, add HTTP port 8081/TCP to the SELinux policy database persistently.

8. On rhcsa1 change the system time to the “America/New_York” timezone.

9. On both VMs attach the RHEL 9 ISO image to the VM and mount it persistently to /repo. Define access to both repositories and confirm.

10. On both VMs, all new users should have a file named ‘CONGRATS’ in their home folder after account creation.

11. On rhcsa2, all user passwords should expire after 90 days and should be at least 9 characters in length.

12. On rhcsa2, create users edwin and santos and make them members of the group dbadmin as a secondary group membership. Also, create users serene and alex and make them members of the group accounting as a secondary group. Ensure that user santos has UID 1234 and cannot start an interactive shell.

13. On rhcsa2, create shared group directories /groups/dbadmin and /groups/accounting, and make sure the groups meet the following requirements:
    - Members of the group dbadmin have full access to their directory.
    - Members of the group accounting have full access to their directory.
    - New files that are created in the group directory are group owned by the group owner of the parent directory.
    - Others have no access to the group directories.

14. On rhcsa2, Find all files that are owned by user edwin and copy them to the directory /root/edwinfiles.

15. Create user bob and set this user’s shell so that this user can only change the password and cannot do anything else.

16. On rhcsa1, list all files that are part of the “setup” package, and use regular expressions and I/O redirection to send the output lines containing “hosts” to /var/tmp/setup.pkg