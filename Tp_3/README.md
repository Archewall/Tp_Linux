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
[alexandre@localhost ssh]$ 

Port 31433
#GatewayPorts no
```

gérer le firewall 
```
[alexandre@localhost ssh]$ sudo firewall-cmd --add-port=31433/tcp --permanent


[alexandre@localhost ssh]$ sudo firewall-cmd --list-all | grep ports
ports: 31433/tcp
```

effectuer une connexion SSH  sur le nouveau port 
```
PS C:\Users\Makhov> ssh alexandre@10.2.1.2 -p 31433
alexandre@10.2.1.2's password:
Last login: Mon Jan 29 14:33:24 2024 from 10.2.1.1
```


# Partie 2 Service WEB 

## II. Service HTTP

install NGINX
```
[alexandre@localhost ~]$ sudo dnf install nginx
```

 Démarrer le service NGINX
 ```powershell
 [alexandre@localhost ~]$ sudo systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; prese>
     Active: active (running) since Tue 2024-01-30 00:54:44 CET; 19s ago
 ```

  Déterminer sur quel port tourne NGINX

  nginc tourne sur le port 80:
  ```powershell
[alexandre@localhost ~]$ ss -saleputn| grep nginx
tcp   LISTEN 0      511          0.0.0.0:80   
  ```
ouvrir le port dans le firewall :
```powershell
[alexandre@localhost ~]$ sudo firewall-cmd --add-port=80/tcp --permanent
```

processus lié au service Nginx:
```powershell
[alexandre@localhost ~]$ ps -ef |grep nginx
root        2950       1  0 00:54 ?        00:00:00 nginx: master process /usr/sbin/nginx
nginx       2951    2950  0 00:54 ?        00:00:00 nginx: worker process
```

Déterminer le nom de l'utilisateur qui lance NGINX : c'est nginx !
```powershell
nginx       2951    2950  0 00:54 ?        00:00:00 nginx: worker process

[alexandre@localhost ~]$ sudo cat /etc/passwd | grep nginx
[sudo] password for alexandre:
nginx:x:991:991:Nginx web server:/var/lib/nginx:/sbin/nologin
```

TEST
```powershell
[alexandre@localhost ~]$ curl http://10.2.1.2:80 | head -n 7
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0<!doctype html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
    <style type="text/css">
```

## 2. Analyser la conf de NGINX

 Déterminer le path du fichier de configuration de NGINX
 ```powershell 
 [alexandre@localhost nginx]$ ls -al nginx.conf
-rw-r--r--. 1 root root 2334 Oct 16 20:00 nginx.conf
 ```

Trouver dans le fichier conf

- Bloc server { }
```powershell
[alexandre@localhost nginx]$ sudo cat nginx.conf | grep server -A 10
    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }
```

une ligne qui commence par include
```powershell
[alexandre@localhost nginx]$ sudo cat nginx.conf | grep include
[sudo] password for alexandre:
include /usr/share/nginx/modules/*.conf;
    include             /etc/nginx/mime.types;

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/default.d/*.conf;
```

## 3. Déployer un nouveau site web

Creer un site web 
```powershell
[alexandre@localhost var]$ sudo mkdir www
[alexandre@localhost var]$ cd www
[alexandre@localhost www]$ sudo mkdir tp3_linux
[alexandre@localhost www]$ cd tp3_linux/
[alexandre@localhost tp3_linux]$ ls
[alexandre@localhost tp3_linux]$ nano index.html
[alexandre@localhost tp3_linux]$ sudo nano index.html
[alexandre@localhost tp3_linux]$ cat index.html
<h1>MEOW mon premier serveur web</h1>
```



gerer les perms
```powershell
[alexandre@localhost tp3_linux]$ sudo chown nginx /var/www/tp3_linux/
```

adapter la conf nginx
```powershell
[alexandre@localhost nginx]$ sudo nano nginx.conf
[alexandre@localhost nginx]$ sudo systemctl restart nginx

[alexandre@localhost /]$ cd etc/
[alexandre@localhost etc]$ cd nginx/conf.d
[alexandre@localhost ~]$ echo $RANDOM
28608
[alexandre@localhost conf.d]$ sudo nano nyah.conf
server {
  # le port choisi devra être obtenu avec un 'echo $RANDOM' là encore
  listen 28608;

  root /var/www/tp3_linux;
}
```

Visitez votre super site web
```powershell
[alexandre@localhost nginx]$ curl http://10.2.1.2:28608
*<h1>MEOW mon premier serveur web</h1>


<p>NYAAAAAAAAAAAAAAAAAAH</P>
```
# PARTIE 3 Net cat maison
## III. Your own services


## Analyse des services existants

### Afficher le fichier de service SSH
```bash
[alexandre@localhost ~]$ systemctl status sshd
● sshd.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/sshd.service;

[alexandre@localhost ~]$ sudo cat /usr/lib/systemd/system/ssh
d.service | grep ExecStart=
[sudo] password for alexandre:
ExecStart=/usr/sbin/sshd -D $OPTIONS
```

### Afficher le fichier de service NGINX
```bash
[alexandre@localhost ~]$ systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service;

[alexandre@localhost ~]$ sudo cat /usr/lib/systemd/system/nginx.service | grep ExecStart=
[sudo] password for alexandre:
ExecStart=/usr/sbin/nginx
```

## Création de service

### Créez le fichier /etc/systemd/system/tp3_nc.service
```bash
[alexandre@localhost ~]$ echo $RANDOM
25286

[alexandre@localhost system]$ sudo nano tp3_nc.service
[Unit]
Description=Super netcat tout fou

[Service]
ExecStart=/usr/bin/nc -l 25286 -k
```


### Indiquer au système qu'on a modifié les fichiers de service

```bash
[alexandre@localhost system]$ sudo systemctl daemon-reload
```

### Démarrer notre service de ouf

```bash
[alexandre@localhost system]$ sudo systemctl start tp3_nc.service
```

### Vérifier que ça fonctionne
```bash
[alexandre@localhost system]$ sudo systemctl status tp3_nc.service
● tp3_nc.service - Super netcat tout fou
     Loaded: loaded (/etc/systemd/system/tp3_nc.service; sta>
     Active: active (running) since Tue 2024-01-30 04:28:34 >
   Main PID: 3474 (nc)

[alexandre@localhost system]$ ss -saleputn| grep nc
tcp   LISTEN 0      10           0.0.0.0:25286      0.0.0.0:*    ino:35629 sk:68 cgroup:/system.slice/tp3_nc.service <->
tcp   LISTEN 0      10              [::]:25286         [::]:*    ino:35628 sk:69 cgroup:/system.slice/tp3_nc.service v6only:1 <->
```

### Les logs de votre service
```bash
[alexandre@localhost system]$ sudo journalctl -xe -u tp3_nc | grep Start
Jan 30 04:28:34 localhost.localdomain systemd[1]: Started Etalon du cul.

[alexandre@localhost system]$ sudo journalctl -xe -u tp3_nc | grep nc
    Subject: A start job for unit tp3_nc.service has finished successfully
     A start job for unit tp3_nc.service has finished successfully.
Jan 30 04:49:12 localhost.localdomain nc[3474]: prout
Jan 30 04:49:14 localhost.localdomain nc[3474]: prout
Jan 30 04:49:16 localhost.localdomain nc[3474]: prout
Jan 30 04:49:17 localhost.localdomain nc[3474]: prout
Jan 30 04:49:23 localhost.localdomain nc[3474]: aaaaah pas la mm chose
Jan 30 04:49:26 localhost.localdomain nc[3474]: caca
Jan 30 04:49:36 localhost.localdomain nc[3474]: foutre de chien$
Jan 30 04:49:39 localhost.localdomain nc[3474]: avion
Jan 30 04:49:44 localhost.localdomain nc[3474]: labrador
```


### S'amuser à kill le processus
```powershell
[alexandre@localhost system]$ ps -ef | grep nc
dbus         663       1  0 Jan29 ?        00:00:00 /usr/bin/dbus-broker-launch --scope system --audit
root        3474       1  0 04:28 ?        00:00:00 /usr/bin/nc -l 25286 -k
alexand+    3534    2870  0 04:58 pts/0    00:00:00 grep --color=auto nc
[alexandre@localhost system]$ sudo kill 3474
[alexandre@localhost system]$ sudo systemctl status tp3_nc.service
○ tp3_nc.service - Super netcat tout fou
     Loaded: loaded (/etc/systemd/system/tp3_nc.service; static)
     Active: inactive (dead)
```

 ### Affiner la définition du service

```bash
[alexandre@localhost system]$ sudo nano /etc/systemd/system/tp3_nc.service
[alexandre@localhost system]$ sudo systemctl daemon-reload
```