# Practice Exam

## Task 1: Managing Repository Access
- Copy the content of the RHEL9.x installation disk to the `/rhel9.iso` file.
- Mount this iso file on the `/repo` directory
- Configure your system to use this local `/repo` directory as a repository, providing access to the BaseOS and AppStream packages.

## Task 2: Using Essential Tools
- Locate all files with a size bigger than 200 MiB. Do not inclyde any directories. Use the `ls -l` command to write of all the files to the directory `/root/bigfiles.txt`

## Task 3: Creating Shell Scripts
- Write a script that can be used as a countdonw time. Make sure it meets the following requirements:
    - The name of the script is `countdown.sh`
    - The script should take a number of minutes as its argument
    - If no argument is provided, the script should prompt the user to enter a value and use to enter a value and use that.
    - While running, the script should print `nn seconds remaining`, where nn is the number of seconds. This should happen every second. The current number of seconds should be stored in a variable `COUNTER`
    - The script should count down seconds to 0. When second 0 is reached, the script should stop and print the message `countdown is now finished`.
    - The script should be copied to the recommended location in the search path of regular users.

## Task 4: Operating Running Systems
- Run the command `sleep infinity` with adjusted priority. Make sure it meets the following requirements:
    - It should get the lowest priority that can be set.
    - It should automatically be started as a background job when the user `student` logs in.
