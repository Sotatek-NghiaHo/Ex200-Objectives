Tasks:

# 1 User/Group Creation

Create a group named sharegroup and the following users:

haruna (with no login shell, not a member of sharegroup),

umar (member of sharegrp)

adoga (with UID 4444 member of sharegroup).

All users should have a password: persward.

Change the password of user adoga to perfect

```bash
[root@redhat9-server-1 ~]# for user in umar haruna adoga; do echo persward | passwd --stdin $user; done
Changing password for user umar.
passwd: all authentication tokens updated successfully.
Changing password for user haruna.
passwd: all authentication tokens updated successfully.
Changing password for user adoga.
passwd: all authentication tokens updated successfully.

[root@redhat9-server-1 ~]# echo persward | passwd --stdin umar 
[root@redhat9-server-1 ~]# echo persward | passwd --stdin haruna
[root@redhat9-server-1 ~]# echo persward | passwd --stdin adoga

```

```bash

[root@redhat9-server-1 ~]# vi /etc/security/pwquality.conf
<
# Minimum acceptable size for the new password (plus one if
# credits are not disabled which is the default). (See pam_cracklib manual.)
# Cannot be set to lower value than 6.
minlen = 8
>

[root@redhat9-server-1 ~]# vi /etc/login.defs 
UMASK           022

# HOME_MODE is used by useradd(8) and newusers(8) to set the mode for new
# home directories.
# If HOME_MODE is not set, the value of UMASK is used to create the mode.
HOME_MODE       0700

# Password aging controls:
#
#       PASS_MAX_DAYS   Maximum number of days a password may be used.
#       PASS_MIN_DAYS   Minimum number of days allowed between password changes.
#       PASS_MIN_LEN    Minimum acceptable password length.
#       PASS_WARN_AGE   Number of days warning given before a password expires.
#
PASS_MAX_DAYS   30
#PASS_MAX_DAYS  99999
PASS_MIN_DAYS   0
PASS_WARN_AGE   7

```

---
# 2 User Password Policies

Enforce password policy to have a minimum length of 8 chars.

Set the max password age to 30 days.
```bash

```

---
# 3 Delete Users and Groups

Remove the user umar from sharegroup

Delete the sharegroup

Delete user haruna with their home directory

```bash
[root@redhat9-server-1 ~]# vi /etc/group
sharegroup:x:50004:adoga

```
```bash
[root@redhat9-server-1 ~]# groupdel sharegroup 
[root@redhat9-server-1 ~]# man userdel 
[root@redhat9-server-1 ~]# ls /home/
adam  adoga  haruna  jacob  maryam  nghiahv  umar
[root@redhat9-server-1 ~]# userdel -r haruna 
[root@redhat9-server-1 ~]# ls /home/
adam  adoga  jacob  maryam  nghiahv  umar

```