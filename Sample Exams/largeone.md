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