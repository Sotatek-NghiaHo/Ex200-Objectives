# Task 1: Reset Root Password

Break into node2 and set a new root password to hoppy.

```bash
(GRUB Menu) -> type e (0-resecure)
linux .... rd.break

-> Ctrl+X || F10
-> 
switch_root:/# mount -o remount,rw /sysroot
switch_root:/# chroot /sysroot
sh-5.1# passwd root
redhat
redhat

-> touch /.autorelabel
exit
exit
```

# Task 2: Tuning Profile Configuration and SELINUX

Check the current recommended tuning profile.

Put SELinux in permissive mode on node2.

On node1 ensure network service is enabled and starts on boot.

```bash
[root@redhat9-server-1 ~]# systemctl status tuned.service 
● tuned.service - Dynamic System Tuning Daemon
     Loaded: loaded (/usr/lib/systemd/system/tuned.service; enabled; preset: enabled)
     Active: active (running) since Sun 2025-08-31 22:33:54 +07; 48min ago
```

```bash
[root@redhat9-server-1 ~]# tuned-adm 
usage: tuned-adm [-h] [--version] [--debug] [--async] [--timeout TIMEOUT] [--loglevel LOGLEVEL]
                 {list,active,off,profile,profile_info,recommend,verify,auto_profile,profile_mode,instance_acquire_devices,get_instances,instance_get_devices}
                 ...
[root@redhat9-server-1 ~]# tuned-adm recommend 
virtual-guest
[root@redhat9-server-1 ~]# man tuned-adm 
[root@redhat9-server-1 ~]# tuned-adm list 
Available profiles:
- accelerator-performance     - Throughput performance based tuning with disabled higher latency STOP states
- aws                         - Optimize for aws ec2 instances
- balanced                    - General non-specialized tuned profile
...
- virtual-guest               - Optimize for running inside a virtual guest
- virtual-host                - Optimize for running KVM guests
Current active profile: balanced


```
Switch
```bash
[root@redhat9-server-1 ~]# tuned-adm profile virtual-guest 
[root@redhat9-server-1 ~]# man tuned-adm 
[root@redhat9-server-1 ~]# tuned-adm active 
Current active profile: virtual-guest
```

```bash
[root@redhat9-server-1 ~]# man -k SELinux | grep mode
getenforce (8)       - get the current mode of SELinux
setenforce (8)       - modify the mode SELinux is running in
[root@redhat9-server-1 ~]# getenforce 
Enforcing
```

```bash
[root@redhat9-server-1 ~]# man -k SELinux | grep set
pam_selinux (8)      - PAM module to set the default security context
sedispatch (8)       - setroubleshoot audit dispatcher for SELinux Messages
setenforce (8)       - modify the mode SELinux is running in
setfiles (8)         - set SELinux file security contexts.
setsebool (8)        - set SELinux boolean value
[root@redhat9-server-1 ~]# setenforce 0
[root@redhat9-server-1 ~]# getenforce 
Permissive
[root@redhat9-server-1 ~]# setenforce 1
[root@redhat9-server-1 ~]# getenforce 
Enforcing
```
Or modify in file
```bash
[root@redhat9-server-1 ~]# vi /etc/selinux/config 
```
-> reboot  
Kết luận
- Reboot thì thay đổi bằng setenforce sẽ mất.
- Nếu muốn giữ sau reboot → phải sửa trong /etc/selinux/config. Ví dụ:
- vi /etc/selinux/config
rồi chỉnh:
- SELINUX=enforcing
- hoặc SELINUX=permissive  

```bash
[root@redhat9-server-1 ~]# systemctl status NetworkManager
● NetworkManager.service - Network Manager
     Loaded: loaded (/usr/lib/systemd/system/NetworkManager.service; enabled; preset: enabled)
     Active: active (running) since Sun 2025-08-31 22:33:52 +07; 1h 13min ago

```





# Task 4: Persistent Journaling

Configure persistent journaling on both servers to retain logs across reboots.

```bash
[root@redhat9-server-1 ~]# mkdir /var/log/journal/
[root@redhat9-server-1 ~]# ls /var/log/journal/
[root@redhat9-server-1 ~]# journalctl --flush
[root@redhat9-server-1 ~]# systemctl restart systemd-journald
[root@redhat9-server-1 ~]# ls /var/log/journal/
d4f0ddb5145a4450b72d2f9aebab7ff6
```
Check / test
```bash
logger "TEST PERSISTENT JOURNALING"
journalctl --verify
journalctl -b -1 | grep "TEST PERSISTENT JOURNALING"

```
Note:
- -b -1 = log của boot trước (chứng minh log survive qua reboot).
- -b 0 = log của boot hiện tại.
- -b -2 = boot trước nữa.

```
journalctl -u sshd
```
→ chỉ hiện log của dịch vụ sshd.service.  
👉 Nếu chỉ quan tâm một dịch vụ, thì journalctl -u <service> sẽ tiện hơn nhiều.

---
# Task 5: Process process scheduling

Start a stress-ng process on node1 with a niceness value of 19.

Adjust the niceness value of the running stress-ng process to 10.

Terminate the stress-ng process.

```bash
[root@redhat9-server-1 ~]# dnf list installed stress-ng
Updating Subscription Management repositories.
Installed Packages
stress-ng.x86_64                                          0.18.06-1.el9                                          @rhel-9-for-x86_64-appstream-rpms
[root@redhat9-server-1 ~]# dnf  install stress-ng -y
```

r → renice (đổi độ ưu tiên của tiến trình)

Gõ r → nhập PID → nhập giá trị nice mới (ví dụ 10).

Nice cao hơn = ưu tiên thấp hơn (process “hiền” hơn).

k → kill (dừng hẳn tiến trình)

Gõ k → nhập PID → nhập signal (thường 15 = TERM, hoặc 9 = KILL,stop(sigstop)).

s → đổi refresh interval

Ví dụ s 1 = update mỗi giây.

SPACE → refresh ngay lập tức.

q → thoát top.

# Task 6: File Permissions & File ACLS

Copy /etc/fstab to /var/tmp.

Set the file owner to root

Ensure /var/tmp/fstab is not executable by anyone.

Configure file ACLs on the copied file to:

User adam: read & write.

User maryam: no access.

All other users: read-only.
```
ps aux | grep stress-ng
```

---
# Task 7: Secure File Transfer

On node1, create a file node1-file.ext and securely copy (scp) it to the home dir of user hadoga on node2.

```bash
[root@redhat9-server-1 ~]# touch node1-file.ext
[root@redhat9-server-1 ~]# scp -v node1-file.ext nghiahv@192.168.38.130:/home/nghiahv/
```