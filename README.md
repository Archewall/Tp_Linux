 # TP3 : Services
 ## I. Service SSH
 ### 1. Analyse du service

service SSH démarré 
```
[alexandre@localhost ~]$ systemctl status

 localhost.localdomain
    State: running
```

Service SSHD
```
[alexandre@localhost ~]$ ps -ef | grep sshd

root         691       1  0 14:31 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root        1336     691  0 14:33 ?        00:00:00 sshd: alexandre [priv]
alexand+    1350    1336  0 14:33 ?        00:00:00 sshd: alexandre@pts/0
```

Commande ss 
```
[alexandre@localhost ~]$ ss | grep ssh

tcp   ESTAB  0      52                         10.2.1.2:ssh           10.2.1.1:52708
```

Log service SSH
```
[alexandre@localhost ~]$ journalctl | grep ssh

Jan 29 14:33:23 localhost.localdomain sshd[1336]: Accepted password for alexandre from 10.2.1.1 port 52708 ssh2

Jan 29 14:33:24 localhost.localdomain sshd[1336]: pam_unix(sshd:session): session opened for user alexandre(uid=1000) by (uid=0)
```

Commande tail 
```
[alexandre@localhost ~]$ sudo tail -n 10 /var/log/secure

Jan 29 15:40:12 localhost sudo[1463]: pam_unix(sudo:session): session opened for user root(uid=0) by alexandre(uid=1000)
Jan 29 15:40:12 localhost sudo[1463]: pam_unix(sudo:session): session closed for user root
```

### 2. Modification du service

le fichier de config de serveur SSH : "sshd_config"

modifier le fichier conf :
```
[alexandre@localhost ssh]$ echo $RANDOM
31433
```

Port d'écoute du serveur SSH
```
[alexandre@localhost ssh]$ sudo cat sshd_config| grep Port

Port 31433
#GatewayPorts no
```

gérer le firewall 
```
[alexandre@localhost ssh]$ sudo firewall-cmd --add-port=31433/tcp --permanent


[alexandre@localhost ssh]$ sudo firewall-cmd --list-all | grep ports
ports: 31433/tcp
```

