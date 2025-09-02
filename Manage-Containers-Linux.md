# 1 Container Image Creation

As user `haruna` on node2 (192.168.70.12), build a container image named web_image using the container file located at http://repo.rhcsa.home/containers/Containerfile.


```bash
[root@redhat9-server-1 ~]# loginctl enable-linger haruna
[haruna@redhat9-server-1 ~]# podman image list
|| podman images
[haruna@redhat9-server-1 ~]# podman ps
[haruna@redhat9-server-1 ~]# podman ps -a

[haruna@node2 ~]$ podman image build -t web_image -f http://repo.rhcsa.home/containers/Containerfile .

[haruna@redhat9-server-1 ~]# podman image list
localhost/web_image latest ...

[haruna@redhat9-server-1 ~]# podman login registry.lab.example.com
Username: admin
Password: redhat321
Login Succeeded!


```

---
# 2 Systemd Rootless Container Service

Deploy haruna_web container with the following:

Map host port 8000 → container port 80.

Map the host directory /opt/user_files to container directory/opt/userfiles. Files should be accessible.

Configure the container as a systemd service to start on boot.

```bash
[haruna@redhat9-server-1 ~]# podman run -d --name haruna_web -p 8080:80 \
-v /opt/user_files:/opt/userfiles:Z web_image

...output omitted...
d970ff062f002a45702b96c0a51d632d93d78ccf63a3af1a01abf70bc4c46616

[haruna@redhat9-server-1 ~]# podman ps
[haruna@redhat9-server-1 ~]# podman exec -it haruna_web /bin/bash
[root@78e /]# ll /opt/userfiles 
[root@78e /]# echo "RHCSA" > rhcsa.txt

[haruna@redhat9-server-1 ~]# cat /opt/user_files/rhcsa.txt
RHCSA

```

Systemd
```bash
[haruna@redhat9-server-1 ~]# podman generate systemd --new --files --name haruna_web

[haruna@redhat9-server-1 ~]# mkdir -p ~/.config/systemd/user
[haruna@redhat9-server-1 ~]# mv container-haruna_web.service ~/.config/systemd/user

[haruna@redhat9-server-1 ~]# cd ~/.config/systemd/user

[haruna@redhat9-server-1 user]# ls
container-haruna_web.service

[haruna@redhat9-server-1 user]# podman stop haruna_web
[haruna@redhat9-server-1 user]# podman rm haruna_web

[haruna@redhat9-server-1 user]# systemctl --user daemon-reload

[haruna@redhat9-server-1 user]# loginctl enable-linger

[haruna@redhat9-server-1 user]# systemctl --user enable --now container-haruna_web.service

[haruna@redhat9-server-1 user]# systemctl --user status --now container-haruna_web.service
[haruna@redhat9-server-1 user]# systemctl --user stop --now container-haruna_web.service
```
