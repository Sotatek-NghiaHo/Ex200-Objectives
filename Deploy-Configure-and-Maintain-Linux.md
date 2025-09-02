# 1 Cron Job Configuration

Create a cron job for user hadoga that runs logger "RHCSA Playlist Now Available" every 2 minutes.

Use at to write "This task was easy!" to /at-files/at.txt in 2 minutes.

```bash
[root@redhat9-server-1 ~]# systemctl status crond.service 
● crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; preset: enabled)
     Active: active (running) since Tue 2025-09-02 08:25:53 +07; 35min ago

[root@redhat9-server-1 ~]# cat /etc/crontab 
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

```

```bash
[root@redhat9-server-1 ~]# crontab -l
no crontab for root
[root@redhat9-server-1 ~]# crontab -l -u nghiahv 
no crontab for nghiahv
```

```bash
[root@redhat9-server-1 ~]# crontab -e -u nghiahv 
no crontab for nghiahv - using an empty one
crontab: installing new crontab
[root@redhat9-server-1 ~]# crontab -l -u nghiahv 
*/2 * * * * logger "RHCSA Playlist NOW Available" 
[root@redhat9-server-1 ~]# grep RHCSA /var/log/messages
Sep  2 09:22:01 redhat9-server-1 nghiahv[6184]: RHCSA Playlist NOW Available
```

```bash
[root@redhat9-server-1 ~]# dnf install at -y
[root@redhat9-server-1 ~]# systemctl status atd.service 
● atd.service - Deferred execution scheduler
     Loaded: loaded (/usr/lib/systemd/system/atd.service; enabled; preset: enabled)
     Active: active (running) since Tue 2025-09-02 09:25:06 +07; 19s ago

[root@redhat9-server-1 ~]# echo "This task was easy!" >> /at-files/at.txt | at now + 2 minutes
warning: commands will be executed using /bin/sh
job 1 at Tue Sep  2 09:30:00 2025
[root@redhat9-server-1 ~]# atq
1	Tue Sep  2 09:30:00 2025 a root
[root@redhat9-server-1 ~]# ls /at-files/
at.txt
[root@redhat9-server-1 ~]# cat /at-files/at.txt 
This task was easy!

```

---
# 2 Local YUM Repository Configuration

Configure BaseOS (URL: http://repo.rhcsa.home/repo/BaseOS/) and AppStream (URL: http://repo.rhcsa.home/repo/AppStream/) repos on node1

```bash
[root@redhat9-server-1 ~]# cd /etc/yum.repos.d/
[root@redhat9-server-1 yum.repos.d]# ls
redhat.repo
[root@redhat9-server-1 yum.repos.d]# vi rhcsa.repo

[BaseOS]
name=BaseOS RHCSA
baseurl=http://repo.rhcsa.home/repo/BaseOs
enable=1
gpgcheck=0

[AppStream]
name=AppStream RHCSA
baseurl=http://repo.rhcsa.home/repo/AppStream/
enable=1
gpgcheck=0

```
Check
```bash
[root@redhat9-server-1 yum.repos.d]# dnf repolist 
Updating Subscription Management repositories.
repo id                                                      repo name
AppStream                                                    AppStream RHCSA
BaseOS                                                       BaseOS RHCSA
```

---
# 3 NTP Chrony Configuration

Set up chrony time service to sync time with server.rhel.com.

```bash
[root@redhat9-server-1 ~]# systemctl status chronyd.service 
● chronyd.service - NTP client/server
     Loaded: loaded (/usr/lib/systemd/system/chronyd.service; enabled; preset: enabled)
     Active: active (running) since Tue 2025-09-02 08:25:49 +07; 2h 0min ago

[root@redhat9-server-1 ~]# vi /etc/chrony.conf 
<
# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (https://www.pool.ntp.org/join.html).
pool 2.rhel.pool.ntp.org iburst
#RHCSA server
pool server.rhel.com iburst
>

[root@redhat9-server-1 ~]# systemctl restart chronyd.service 

[root@redhat9-server-1 ~]# chrony
chronyc  chronyd  
[root@redhat9-server-1 ~]# chronyc source
sourcename   sources      sourcestats  
[root@redhat9-server-1 ~]# chronyc sources
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^* play.xtdv.gr                  2   6    47    26  +1181us[+1072us] +/-   34ms
^- 115.165.161.155               2   6    63    22  +1097us[+1097us] +/-   32ms
```


---
# 4 GRUB Bootloader Modification

Set GRUB_TIMEOUT=10,

GRUB_TIMEOUT_STYLE=hidden, and add quiet to GRUB_CMDLINE_LINUX.

Apply your changes to the grub config file.

```bash
[root@redhat9-server-1 ~]# vi /etc/default/grub 

<
GRUB_TIMEOUT=10

GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"

GRUB_DEFAULT=saved

GRUB_DISABLE_SUBMENU=true

GRUB_TERMINAL_OUTPUT="console"

GRUB_CMDLINE_LINUX="crashkernel=1G-4G:256M,4G-64G:320M,64G-:576M rd.lvm.lv=rhel/root rd

.lvm.lv=rhel/swap quiet"

GRUB_DISABLE_RECOVERY="true"

GRUB_ENABLE_BLSCFG=true

GRUB_TIMEOUT_STYLE=hidden
>

```

Apply changes
```bash
[root@redhat9-server-1 ~]# grub2-mkconfig -o /boot/grub2/grub.cfg 
Generating grub configuration file ...
Adding boot menu entry for UEFI Firmware Settings ...
done
[root@redhat9-server-1 ~]# 


```

Note:
- Sửa trong file theo yêu cầu
- Mục nào không có thì thêm vào