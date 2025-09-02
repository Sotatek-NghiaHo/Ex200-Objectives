# Task 1: Text Search & Archive - Man pages

**1.1 Find the string "Listen" in /etc/httpd/conf/httpd.conf and save the output to /root/web.txt*
```bash
[root@redhat9-server-1 ~]# grep Listen /etc/httpd/conf/httpd.conf >> /root/web.txt
[root@redhat9-server-1 ~]# cat web.txt 
# Listen: Allows you to bind Apache to specific IP addresses and/or
# Change this to Listen on a specific IP address, but note that if
#Listen 12.34.56.78:80
Listen 80
```


*1.2 Create a gzip-compressed tar archive of /etc named etc_vault.tar.gz in /vaults directory.*
(use man pages for gzip option)
```bash
[root@redhat9-server-1 ~]# mkdir /vaults
[root@redhat9-server-1 ~]# man tar  (/gzip -> z )
[root@redhat9-server-1 ~]# tar cvfz /vaults/etc_vaults.tar.gz /etc/
tar: Removing leading `/' from member names
/etc/
/etc/mtab
/etc/fstab

[root@redhat9-server-1 ~]# ls /vaults/
etc_vaults.tar.gz
```


---
# Task 2: File Links - Shortcuts

2.1 In/shorts directory:

Create a file file_a

Create soft link file_b pointing to file_a

Create hard link file_c pointing to file_a

Verify all links work
```bash
[root@redhat9-server-1 ~]# mkdir /shorts
[root@redhat9-server-1 ~]# touch /shorts/file_a
[root@redhat9-server-1 ~]# man ln
[root@redhat9-server-1 ~]# cd /shorts/
[root@redhat9-server-1 shorts]# ls
file_a
[root@redhat9-server-1 shorts]# ln -s file_a file_b
[root@redhat9-server-1 shorts]# ln file_a file_c
[root@redhat9-server-1 shorts]# ll
total 0
-rw-r--r--. 2 root root 0 Aug 31 16:37 file_a
lrwxrwxrwx. 1 root root 6 Aug 31 16:37 file_b -> file_a
-rw-r--r--. 2 root root 0 Aug 31 16:37 file_c
[root@redhat9-server-1 shorts]# ll -i
total 0
53239675 -rw-r--r--. 2 root root 0 Aug 31 16:37 file_a
53268813 lrwxrwxrwx. 1 root root 6 Aug 31 16:37 file_b -> file_a
53239675 -rw-r--r--. 2 root root 0 Aug 31 16:37 file_c

[root@redhat9-server-1 shorts]# echo "File testting" >> file_a
[root@redhat9-server-1 shorts]# cat file_b
File testting
[root@redhat9-server-1 shorts]# cat file_c
File testting
```


---
# Task 3: Advanced File Operations - Find

3.1. Find files in /usr that are greater than 3MB but < 10MB and copy them to /bigfiles directory.
```bash
[root@redhat9-server-1 ~]# man find (/size)
[root@redhat9-server-1 ~]# # find /usr -type f -size +3M -size -10M exec {}  \;
[root@redhat9-server-1 ~]# mkdir /bigfiles
[root@redhat9-server-1 ~]# find /usr -type f -size +3M -size -10M -exec cp {} /bigfiles/  \;
[root@redhat9-server-1 ~]# ls /bigfiles/
20-pci-vendor-model.hwdb   kheaders.ko.xz            libsystemd-shared-252.so              mysqlcheck                           ptp
adsp.mbn.xz                left_ptr_watch            linux.words                           mysqldump                            pw_dict.pwd
...
```



3.2 Find files in /etc modified more than 120 days ago and copy them to /var/tmp/twenty/
```bash
[root@redhat9-server-1 ~]# mkdir /var/tmp/twenty/
[root@redhat9-server-1 ~]# find /etc/ -type f -mtime +120 -exec cp {} /var/tmp/twenty/ \;
[root@redhat9-server-1 ~]# ls /var/tmp/twenty/
000-shortnames.conf                      f18.kti                                             org.gnome.SettingsDaemon.MediaKeys.desktop                                      
```

3.3 Find all files owned by user hadoga and copy them to /root/h-files
```bash
[root@redhat9-server-1 ~]# mkdir h-files
[root@redhat9-server-1 ~]# find / -type f -user nghiahv -group nghiahv -exec cp {} /root/h-files/ \;
find: ‘/proc/5958/task/5958/fdinfo/5’: No such file or directory
find: ‘/proc/5958/fdinfo/6’: No such file or directory
[root@redhat9-server-1 ~]# ls h-files/
season1_project_plan.odf
season2_project_plan.odf
season2_project_plan.odf.back
```
3.4 Find a file named "httpd.conf" and save the absolute paths to /root/httpd-paths.txt
```bash
[root@redhat9-server-1 ~]# find / -type f -name httpd.conf >> /root/httpd-paths.txt
[root@redhat9-server-1 ~]# cat httpd-paths.txt 
/etc/httpd/conf/httpd.conf
/var/tmp/twenty/httpd.conf
/usr/lib/tmpfiles.d/httpd.conf
/usr/lib/sysusers.d/httpd.conf
```
---
# Task 4: Remote Access & File Permissions

4.1 From node1, SSH into node2 as user `hadoga` and 
- 4.2 Copy the contents of /etc/fstab to /var/tmp
- 4.3 Set the file ownership to root
- 4.4 Ensure no execute permissions for anyone

```bash
[root@redhat9-server-1 ~]# hostname
redhat9-server-1
[root@redhat9-server-1 ~]# cp /etc/fstab /var/tmp/
[root@redhat9-server-1 ~]# ls /var/tmp/
dnf-dbadmin1-0sv5stz3
dnf-nghiahv-154jjrxh
fstab

[root@redhat9-server-1 ~]# ll /var/tmp/fstab 
-rw-r--r--. 1 root root 670 Aug 31 17:01 /var/tmp/fstab
[root@redhat9-server-1 ~]# chown root: /var/tmp/fstab 
[root@redhat9-server-1 ~]# ll /var/tmp/fstab 
-rw-r--r--. 1 root root 670 Aug 31 17:01 /var/tmp/fstab
[root@redhat9-server-1 ~]# chmod -x /var/tmp/fstab 
[root@redhat9-server-1 ~]# ll /var/tmp/fstab 
-rw-r--r--. 1 root root 670 Aug 31 17:01 /var/tmp/fstab
```