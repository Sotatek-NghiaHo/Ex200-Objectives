
# 1 Configure Key-based Authentication as root & Allow SSH Root Access

Generate SSH key on node1, and setup key-based authentication for the root user.

Enable SSH root login and test your configuration.

```bash
ssh-keygen
ls .ssh/

vi /etc/ssh/sshd_config
PermitRootLogin yes

systemctl restart sshd

```
Check
```bash
ssh-copy-id root@<IP_NODE2>
ssh root@<IP_NODE2>

```

---
# 2 HTTPD Server Troubleshooting SELINUX

The web server on node1 (192.168.70.10) is listening on port 85, ensure that the website is reachable via that port.

Fix SELinux context for /var/www/html, if needed.

Test using curl http://server-ip/ or use a web browser


```bash
[root@redhat9-server-1 ~]# systemctl status httpd.service 

[root@redhat9-server-1 ~]# vi /etc/httpd/conf/
httpd.conf  magic       
[root@redhat9-server-1 ~]# vi /etc/httpd/conf/httpd.conf

Listen 85

[root@redhat9-server-1 ~]# semanage port -l | grep http
http_cache_port_t              tcp      8080, 8118, 8123, 10001-10010
http_cache_port_t              udp      3130
http_port_t                    tcp      1001, 80, 81, 443, 488, 8008, 8009, 8443, 9000
pegasus_http_port_t            tcp      5988
pegasus_https_port_t           tcp      5989
[root@redhat9-server-1 ~]# man semanage 
[root@redhat9-server-1 ~]# man semanage port
[root@redhat9-server-1 ~]# semanage port -a -t http_port_t -p tcp 85  
[root@redhat9-server-1 ~]# semanage port -l | grep http
http_cache_port_t              tcp      8080, 8118, 8123, 10001-10010
http_cache_port_t              udp      3130
http_port_t                    tcp      85, 1001, 80, 81, 443, 488, 8008, 8009, 8443, 9000
pegasus_http_port_t            tcp      5988
pegasus_https_port_t           tcp      5989

[root@redhat9-server-1 ~]# man firewall-cmd 
[root@redhat9-server-1 ~]# firewall-cmd --permanent --add-port=443/tcp
[root@redhat9-server-1 ~]# firewall-cmd --permanent --add-service http
[root@redhat9-server-1 ~]# firewall-cmd --reload
[root@redhat9-server-1 ~]# firewall-cmd --list-all
```

```bash
systemctl restart httpd.service
curl http://ip:85
```