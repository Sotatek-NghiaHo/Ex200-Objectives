# Task 1: Size-Based File Search

Create a shell script that:

1 Finds files in /usr sized >30KB but <50KB  
2 Outputs results to /root/sized_files.txt

```
[root@redhat9-server-1 ~]# mkdir scripts
[root@redhat9-server-1 ~]# cd scripts
[root@redhat9-server-1 scripts]# which bash
/usr/bin/bash
[root@redhat9-server-1 scripts]# vi find-files.sh
```
```bash
#!/usr/bin/bash

# Scipts to find files

find /usr -type f -size +30k -size -50k > /root/sized_files.txt
```
```bash
[root@redhat9-server-1 scripts]# ls
find-files.sh
[root@redhat9-server-1 scripts]# chmod +x find-files.sh 
[root@redhat9-server-1 scripts]# ls
find-files.sh
[root@redhat9-server-1 scripts]# ./find-files.sh 
[root@redhat9-server-1 scripts]# cat /root/sized_files.txt 
/usr/bin/fusermount
/usr/bin/cd-info
```


---
# Task 2: Conditional Script (career.sh) with argument

Create /root/career.sh that:

1 Outputs "Yes, I'm a Systems Engineer." when run with ./career.sh me

2 Outputs "Okay, they do cloud engineering." when run with ./career.sh they

3 Outputs "Usage: ./career.sh me they" for invalid/empty arguments

```bash
[root@redhat9-server-1 scripts]# vi career.sh
[root@redhat9-server-1 scripts]# ./career.sh 
Please provide an argument: me|they
[root@redhat9-server-1 scripts]# ./career.sh me
Yes, I'm a Systems Engineer.
[root@redhat9-server-1 scripts]# ./career.sh they
Okay, they do cloud engineering.
[root@redhat9-server-1 scripts]# ./career.sh asdsad
Usage: ./career.sh me|they
```

```bash
[root@redhat9-server-1 scripts]# cat career.sh 
#!/usr/bin/bash
# Script to check career

if [ "$1" == "me" ]
then
	echo "Yes, I'm a Systems Engineer."
elif [ "$1" == "they" ]
then
	echo "Okay, they do cloud engineering."
elif [ -z "$1" ]
then
	echo "Please provide an argument: me|they"
else 
	echo "Usage: ./career.sh me|they"
fi

[root@redhat9-server-1 scripts]# chmod +x career.sh 

```


---
# Task 3: File processing, input/output Users and Groups Script

Write shell scripts on nodel that create users and groups according to the following parameters.

maryam:2030:hpc_admin,hpc_managers

adam:2040:sysadmin,

jacob:2050:hpc_admin

Write a shell script that sets the passwords of the users maryam, adam and jacob to Password@1

```bash
[root@redhat9-server-1 scripts]# cat groups.txt 
hpc_admin:9090
hpc_managers:8080
sysadmin:7070
```


```bash
[root@redhat9-server-1 scripts]# chmod +x create_groups.sh 

[root@redhat9-server-1 scripts]# cat create_groups.sh 
#!/usr/bin/bash

# 

while IFS=":" read group gid
do
	echo "Creating group $group with GID $gid"
	groupadd -g $gid $group
done < groups.txt
```

```bash
[root@redhat9-server-1 scripts]# tail /etc/group
hpc_admin:x:9090:
hpc_managers:x:8080:
sysadmin:x:7070:
```

```bash
[root@redhat9-server-1 scripts]# cat create_users.sh 
#!/usr/bin/bash

#

while IFS=":" read user uid group
do 
	echo "Creating user $user with UID $uid belonging to groups $groups"
	useradd -G $groups -u $uid $user
done < users.txt

[root@redhat9-server-1 scripts]# cat users.txt 
maryam:2030:hpc_admin,hpc_managers
adam:2040:sysadmin
jacob:2050:hpc_admin

[root@redhat9-server-1 scripts]# chmod +x create_users.sh 

[root@redhat9-server-1 scripts]# ./create_users.sh 
Creating user maryam with UID 2030 belonging to groups 
Creating user adam with UID 2040 belonging to groups 
Creating user jacob with UID 2050 belonging to groups 
```
Check
```bash
[root@redhat9-server-1 scripts]# tail /etc/passwd
maryam:x:2030:2030::/home/maryam:/bin/bash
adam:x:2040:2040::/home/adam:/bin/bash
jacob:x:2050:2050::/home/jacob:/bin/bash
[root@redhat9-server-1 scripts]# tail /etc/group
hpc_admin:x:9090:maryam,jacob
hpc_managers:x:8080:maryam
sysadmin:x:7070:adam
```

```bash
[root@redhat9-server-1 scripts]# vi setpass.sh
[root@redhat9-server-1 scripts]# chmod +x setpass.sh 
[root@redhat9-server-1 scripts]# ./setpass.sh 
Changing password for user maryam.
passwd: all authentication tokens updated successfully.
Changing password for user adam.
passwd: all authentication tokens updated successfully.
Changing password for user jacob.
passwd: all authentication tokens updated successfully.
[root@redhat9-server-1 scripts]# cat setpass.sh 
#!/usr/bin/bash

# Set password for user

for user in maryam adam jacob
do
	echo Password@1 | passwd --stdin $user
done
```
