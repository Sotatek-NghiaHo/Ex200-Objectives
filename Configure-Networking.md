Tasks on node1:

# 1 Enable Network Services

Ensure network services starts at boot.

```bash
[root@redhat9-server-1 ~]# systemctl status NetworkManager
● NetworkManager.service - Network Manager
     Loaded: loaded (/usr/lib/systemd/system/NetworkManager.service; enabled; preset: enabled)
     Active: active (running) since Tue 2025-09-02 08:25:53 +07; 2h 18min ago


```
# 2 Firewall Rules

Allow access SSH and HTTP services using firewall-cmd.
```bash
[root@redhat9-server-1 ~]# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens160
  sources: 
  services: cockpit dhcpv6-client ssh
  ports: 1001/tcp 80/tcp
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 

[root@redhat9-server-1 ~]# firewall-cmd --help | grep add | grep port


```
Trong firewalld bạn có 2 cách để mở cho phép truy cập:
```bash
firewall-cmd --permanent --add-service=ssh
firewall-cmd --permanent --add-service=http
firewall-cmd --reload
firewall-cmd --list-all
```
Or
```bash
firewall-cmd --permanent --add-port=22/tcp
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --reload
```

---
# 3 Static IP Configuration

Assign IP 172.16.1.10/24, gateway 172.16.1.1, DNS 172.16.1.1,

hostname node5.rhcsa.

Set DNS search domain to rhel.server.com.

**Cách 1: Dùng nmtui**  
Chạy công cụ:

nmtui


Vào Edit a connection → chọn card mạng (ví dụ ens160 hay eth0).

Sửa cấu hình:

IPv4 CONFIGURATION → chọn Manual.
```
Address: 172.16.1.10/24

Gateway: 172.16.1.1

DNS servers: 172.16.1.1

Search domain: rhel.server.com
```
Lưu và thoát.

Đặt hostname: Trong nmtui, chọn Set system hostname → nhập node5.rhcsa.

Restart network:

nmcli connection down <tên-card> && nmcli connection up <tên-card>

**Cách 2: Dùng dòng lệnh**  
Giả sử card mạng là ens160 (bạn kiểm tra bằng nmcli con show).

1. Đặt IP, subnet, gateway, DNS, search domain
nmcli con mod ens160 ipv4.addresses 172.16.1.10/24
nmcli con mod ens160 ipv4.gateway 172.16.1.1
nmcli con mod ens160 ipv4.dns "172.16.1.1"
nmcli con mod ens160 ipv4.dns-search "rhel.server.com"
nmcli con mod ens160 ipv4.method manual

2. Đặt hostname
hostnamectl set-hostname node5.rhcsa

3. Khởi động lại kết nối
nmcli con down ens160 && nmcli con up ens160

4. Kiểm tra
ip a
nmcli dev show ens160 | grep DNS
hostnamectl

