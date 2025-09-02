# RH124

**15.2 Lab: Manage Files from the Command Line**

Manage files, redirect a specific set of lines from a text file to another file, and edit the text files.

Outcomes
- Manage files from the command line.
- Display a specific number of lines from text files and redirect the output to another file.
- Edit text files.

If you did not reset your `workstation` and server machines at the end of the last chapter, then save any work that you want to keep from earlier exercises on those machines, and reset them now.

As the `student` user on the `workstation` machine, use the lab command to prepare your system for this exercise.

This command prepares your environment and ensures that all required resources are available.
```bash
[student@workstation ~]$ lab start rhcsa-rh124-review1
```
*Specifications*
- Log in to `serverb` as the `student` user.
- Create the `/home/student/grading` directory.
- Create three empty files called grade1, grade2, and grade3, in the `/home/student/grading` directory.
- Capture the first five lines of the` /home/student/bin/manage` file in the` /home/student/grading/review.txt file`.
- Append the last three lines of the /home/student/bin/manage file to the /home/student/grading/review.txt file. Do not overwrite any existing text in the /home/student/grading/review.txt file.
- Copy the /home/student/grading/review.txt file to the `/home/student/grading/review-copy.txt` file.
- Edit the `/home/student/grading/review-copy.txt` file so that the Test JJ line appears twice.
- Edit the `/home/student/grading/review-copy.txt` file to remove the Test HH line.
- Edit the `/home/student/grading/review-copy.txt` file so that a line with A new line exists between the Test BB line and the Test CC line.
- Create the `/home/student/hardcopy` hard link to the `/home/student/grading/grade1 file`. You must create the hard link after completing the earlier step to create the `/home/student/grading/grade1` file.
- Create the `/home/student/softcopy` symbolic link to the `/home/student/grading/grade2` file.
- Save the output of a command that lists the contents of the /boot directory to the `/home/student/grading/longlisting.txt` file. The output should be a "long listing" that includes file permissions, owner and group owner, size, and modification date of each file. The output should omit hidden files.

End
```bash
[student@workstation ~]$ lab grade rhcsa-rh124-review1
[student@workstation ~]$ lab finish rhcsa-rh124-review1
```

**15.3 Lab: Manage Users and Groups, Permissions, and Processes**

Manage user and group accounts, set permissions on files and directories, and manage processes.

Outcomes
- Manage user accounts and groups.
- Set permissions on files and directories.
- Identify and manage high CPU-consuming processes.


If you did not reset your `workstation` and server machines at the end of the last chapter, then save any work that you want to keep from earlier exercises on those machines, and reset them now.

As the `student` user on the `workstation` machine, use the lab command to prepare your system for this exercise.

This command prepares your environment and ensures that all required resources are available.
```bash
[student@workstation ~]$ lab start rhcsa-rh124-review2
```
*Specifications*
- Log in to `serverb` as the `student` user.
- Identify and terminate the process that currently uses the most CPU time.
- Create the database group with a GID of 50000.
- Create the dbadmin1 user and configure it with the following requirements:
- Add the database group as a supplementary group.
- Set the password to redhat and force a password change on first login.
- Allow the password to change after 10 days since the day of the last password change.
- Set the password expiration to 30 days since the day of the last password change.
- Allow the user to use the sudo command to run any command as the superuser.
- Configure the default umask as 007 for the dbadmin1 user.
- Create the /home/dbadmin1/grading/review2 directory with dbadmin1 as the owning user and the database group as the owning group.
- Configure the /home/dbadmin1/grading/review2 directory so that the database group owns any file or sub-directory that is created in this directory, irrespective of which user created the file. Configure the permissions on the directory to allow members of the database group to access the directory and to create contents in it. All other users should have read and execute permissions on the directory.
- Ensure that users are allowed to delete only files that they own from the /home/dbadmin1/grading/review2 directory.

```bash
[student@workstation ~]$ lab grade rhcsa-rh124-review2
[student@workstation ~]$ lab finish rhcsa-rh124-review2
```

**15.4 Lab: Configure and Manage a Server**

Configure, secure, and use the SSH service to access a remote machine, and manage packages with the dnf utility.

Outcomes
- Create a new SSH key pair.

- Disable SSH logins as the `root` user.

- Disable password-based SSH logins.

- Install packages and package modules by using the dnf command.


If you did not reset your `workstation` and server machines at the end of the last chapter, then save any work that you want to keep from earlier exercises on those machines, and reset them now.

As the `student` user on the `workstation` machine, use the lab command to prepare your system for this exercise.

This command prepares your environment and ensures that all required resources are available.
```
[student@workstation ~]$ lab start rhcsa-rh124-review3
```
*Specifications*

- Log in to `serverb` as the `student` user.

- Generate SSH keys for the `student` user. Do not protect the private key with a passphrase. Save the private and public keys as the /home/student/.ssh/review3_key and /home/student/.ssh/review3_key.pub files respectively.

- Configure the `student` user on servera to accept logins that are authenticated by the review3_key SSH key pair. The `student` user on `serverb` should be able to log in to servera by using SSH without entering a password.

- On `serverb`, configure the sshd service to prevent the `root` user from logging in.

- On `serverb`, configure the sshd service to prevent users from using their passwords to log in. Users should still be able to authenticate logins by using an SSH key pair.

- Install the zsh package on the `serverb` machine.

```bash
[student@workstation ~]$ lab grade rhcsa-rh124-review3
[student@workstation ~]$ lab finish rhcsa-rh124-review3
```

**15.5 Lab: Manage Networks**  
Configure and test network connectivity.

Outcomes
- Configure network settings.
- Test network connectivity.
- Set a static hostname.
- Use locally resolvable canonical hostnames to connect to systems.


If you did not reset your `workstation` and server machines at the end of the last chapter, then save any work that you want to keep from earlier exercises on those machines, and reset them now.

As the `student` user on the `workstation` machine, use the lab command to prepare your system for this exercise.

This command prepares your environment and ensures that all required resources are available.
```
[student@workstation ~]$ lab start rhcsa-rh124-review4
```
*Important*  
When you use the ssh command to adjust networking settings, an incorrect command might hang or lock out your session. You are then disconnected from the server, and thus the server becomes inaccessible. You must adjust the network configuration through the server console, locally or through a remote console.

In this review, open the `serverb` machine console to adjust the networking settings.

*Specifications*

- On `serverb`, determine the name of the Ethernet interface and its active connection profile.

- On `serverb`, create and activate a static connection profile for the available Ethernet interface. The static profile statically sets network settings and does not use DHCP. Configure the static profile to use the network settings in the following table:

Parameter	|Setting
---|---
IPv4 address	|172.25.250.111
Netmask	|255.255.255.0
Gateway	|172.25.250.254
DNS Server	|172.25.250.254

- Set the `serverb` hostname to server-review4.lab4.example.com.

- On `serverb`, set client-review4 as the canonical hostname for the servera 172.25.250.10 IPv4 address.

- Configure the static connection profile with an additional IPv4 address of 172.25.250.211 with a netmask of 255.255.255.0. Do not remove the existing IPv4 address. Ensure that `serverb` responds to all addresses when the static connection is active.

- On `serverb`, restore the original network settings by activating the original network connection profile.

```bash
[student@workstation ~]$ lab grade rhcsa-rh124-review4
[student@workstation ~]$ lab finish rhcsa-rh124-review4
```

**15.6 Lab: Mount File Systems and Find Files**

Mount a file system and locate files based on different criteria.

Outcomes

- Mount an existing file system.
- Find files based on their file name, permissions, and size.


If you did not reset your `workstation` and server machines at the end of the last chapter, then save any work that you want to keep from earlier exercises on those machines, and reset them now.

As the ``student`` user on the `workstation` machine, use the lab command to prepare your system for this exercise.

This command prepares your environment and ensures that all required resources are available.
```
[student@workstation ~]$ lab start rhcsa-rh124-review5
```
*Specifications*

- Log in to the `serverb` machine as the `student` user and switch to the `root` user.

- Identify the unmounted block device that contains an XFS file system on the `serverb` machine. Mount the block device on the /review5-disk directory.

- Find the review5-path file. Create the /review5-disk/review5-path.txt file that contains a single line with the absolute path to the review5-path file.

- Find all the files that the contractor1 user and the contractor group own. The files must also have the 640 octal permissions. Save the list of these files in the /review5-disk/review5-perms.txt file.

- Find all files with a size of 100 bytes. Save the absolute paths of these files in the /review5-disk/review5-size.txt file.


```bash
[student@workstation ~]$ lab grade rhcsa-rh124-review5
[student@workstation ~]$ lab finish rhcsa-rh124-review5
```

---
# RH134

**14.2 Lab: Fix Boot Issues and Maintain Servers**

Troubleshoot and repair boot problems and update the system default target. You also schedule tasks to run on a repeating schedule as a normal user.

Outcomes
- Diagnose issues and recover the system from emergency mode.
- Change the default target from graphical.target to multi-user.target.
- Schedule recurring jobs to run as a normal user.


If you did not reset your `workstation` and server machines at the end of the last chapter, then save any work that you want to keep from earlier exercises on those machines, and reset them now.

As the `student` user on the `workstation` machine, use the lab command to prepare your system for this exercise.

This command prepares your environment and ensures that all required resources are available.
```bash
[student@workstation ~]$ lab start rhcsa-compreview1
```
*Specifications*
- On `workstation`, run the /tmp/rhcsa-break1 script. This script causes an issue with the boot process on `serverb` and then reboots the machine. Troubleshoot the cause and repair the boot issue. When prompted, use redhat as the password of the `root` user.
- On `workstation`, run the /tmp/rhcsa-break2 script. This script causes the default target to switch from the multi-user target to the graphical target on the `serverb` machine and then reboots the machine. On `serverb`, reset the default target to use the multi-user target. The default target settings must persist after reboot without manual intervention. As the `student` user, use the sudo command for performing privileged commands. Use `student` as the password, when required.
- On `serverb`, schedule a recurring job as the `student` user that executes the /home/student/backup-home.sh script hourly between 7 PM and 9 PM every day except on Saturday and Sunday. Download the backup script from http://materials.example.com/labs/backup-home.sh. The backup-home.sh script backs up the /home/`student` directory from `serverb` to servera in the /home/student/serverb-backup directory. Use the backup-home.sh script to schedule the recurring job as the `student` user. Run the command as an executable.
- Reboot the `serverb` machine and wait for the boot to complete before grading.

End
```bash
[student@workstation ~]$ lab grade rhcsa-compreview1
[student@workstation ~]$ lab finish rhcsa-compreview1
```

**14.3 Lab: Configure and Manage File Systems and Storage**

Create a logical volume, mount a network file system, and create a swap partition that is automatically activated at boot. You also configure directories to store temporary files.

Outcomes
- Create a logical volume.
- Mount a network file system.
- Create a swap partition that is automatically activated at boot.
- Configure a directory to store temporary files.


If you did not reset your `workstation` and server machines at the end of the last chapter, then save any work that you want to keep from earlier exercises on those machines, and reset them now.

As the `student` user on the `workstation` machine, use the lab command to prepare your system for this exercise.

This command prepares your environment and ensures that all required resources are available.

```bash
[student@workstation ~]$ lab start rhcsa-compreview2
```
*Specifications*

- On `serverb`, configure a new 1 GiB vol_home logical volume in a new 2 GiB extra_storage volume group. Use the unpartitioned /dev/vdb disk to create the partition.

- Format the vol_home logical volume with the XFS file-system type, and persistently mount it on the /user-homes directory.

- On `serverb`, persistently mount the /share network file system that servera exports on the /local-share directory. The servera machine exports the servera.lab.example.com:/share path.

- On `serverb`, create a 512 MiB swap partition on the /dev/vdc disk. Persistently mount the swap partition.

- Create the production user group. Create the production1, production2, production3, and production4 users with the production group as their supplementary group.

- On `serverb`, configure the /run/volatile directory to store temporary files. If the files in this directory are not accessed for more than 30 seconds, then the system automatically deletes them. Set 0700 as the octal permissions for the directory. Use the /etc/tmpfiles.d/volatile.conf file to configure the time-based deletion of the files in the /run/volatile directory.

End
```bash
[student@workstation ~]$ lab grade rhcsa-compreview2
[student@workstation ~]$ lab finish rhcsa-compreview2
```

---
**14.4 Lab: Configure and Manage Server Security**
Configure SSH key-based authentication, change firewall settings, adjust the SELinux mode and an SELinux Boolean, and troubleshoot SELinux issues.

Outcomes

- Configure SSH key-based authentication.
- Configure firewall settings.
- Adjust the SELinux mode and SELinux Booleans.
- Troubleshoot SELinux issues.

If you did not reset your `workstation` and server machines at the end of the last chapter, then save any work that you want to keep from earlier exercises on those machines, and reset them now.  
As the `student` user on the `workstation` machine, use the lab command to prepare your system for this exercise.  
This command prepares your environment and ensures that all required resources are available.

```bash
[student@workstation ~]$ lab start rhcsa-compreview3
```
*Specifications*

- On `serverb`, generate an SSH key pair for the `student` user. Do not protect the private key with a passphrase.

- Configure the `student` user on servera to accept login authentication with the SSH key pair that you generated on the `serverb` machine. The `student` user on `serverb` must be able to log in to servera via SSH without entering a password.

- On servera, check the /user-homes/production5 directory permissions. Then, configure SELinux to run in the permissive mode by default.
- On `serverb`, verify that the /localhome directory does not exist. Then, configure the production5 user's home directory to mount the /user-homes/production5 network file system. The servera.lab.example.com machine exports the file system as the servera.lab.example.com:/user-homes/production5 NFS share. Use the autofs service to mount the network share. Verify that the autofs service creates the /localhome/production5 directory with the same permissions as on servera.
- On `serverb`, adjust the appropriate SELinux Boolean so that the production5 user may use the NFS-mounted home directory after authenticating with an SSH key. If required, use redhat as the password of the production5 user.
- On `serverb`, adjust the firewall settings to block all connection requests from the servera machine. Use the servera IPv4 address (172.25.250.10) to configure the firewall rule.
- On `serverb`, investigate and fix the issue with the failing Apache web service, which listens on port 30080/TCP for connections. Adjust the firewall settings appropriately so that the port 30080/TCP is open for incoming connections.

End
```bash
[student@workstation ~]$ lab grade rhcsa-compreview3
[student@workstation ~]$ lab finish rhcsa-compreview3
```

---
**14.5 Lab: Run Containers**

Create `root`less detached containers.

Outcomes

- Create `root`less detached containers.
- Configure port mapping and persistent storage.
- Configure a container as a systemd service and use systemctl commands to manage it.


If you did not reset your `workstation` and server machines at the end of the last chapter, then save any work that you want to keep from earlier exercises on those machines, and reset them now.

As the `student` user on the `workstation` machine, use the lab command to prepare your system for this exercise.

This command prepares your environment and ensures that all required resources are available.
```
[student@workstation ~]$ lab start rhcsa-compreview4
```
*Specifications*
- On `serverb`, configure the podmgr user with redhat as the password, and set up the appropriate tools for the podmgr user to manage the containers for this comprehensive review. Configure registry.lab.example.com as a remote registry. Use admin as the user and redhat321 as the password to authenticate to the registry. You can use the /tmp/review4/registries.conf file to configure the registry.
- The /tmp/review4/container-dev directory contains two directories with development files for the containers in this comprehensive review. Copy the two directories under the /tmp/review4/container-dev directory to the podmgr home directory. Configure the /home/podmgr/storage/database subdirectory so that you can use it as persistent storage for a container.
- Create the db-app01 detached container based on the registry.lab.example.com/rhel9/mariadb-105 container image. Use the /home/podmgr/storage/database directory as persistent storage for the /var/lib/mysql/data directory of the db-app01 container. Map the 13306 port on the local machine to the 3306 port in the container. Use the values of the following table to set the environment variables to create the containerized database:

Variable	|Value
---|---
MYSQL_USER	|developer
MYSQL_PASSWORD	|redhat
MYSQL_DATABASE	|inventory
MYSQL_`root`_PASSWORD	|redhat

- Create a systemd service file to manage the db-app01 container. Configure the systemd service so that when you start the service, the systemd daemon keeps the original container. Start and enable the container as a systemd service. Configure the db-app01 container to start at system boot.

- Copy the /home/podmgr/db-dev/inventory.sql script into the /tmp directory of the db-app01 container, and execute the script inside the container. If you executed the script locally, then you would use the mysql -u `root` inventory < /tmp/inventory.sql command.

- Use the container file in the /home/podmgr/http-dev directory to create the http-app01 detached container. The container image name must be http-client:9.0. Map the 8080 port on the local machine to the 8080 port in the container.

- Use the curl command to query the content of the http-app01 container. Verify that the output of the command shows the container name of the client and that the status of the database is up.

End
```bash
[student@workstation ~]$ lab grade rhcsa-compreview4
[student@workstation ~]$ lab finish rhcsa-compreview4
```


