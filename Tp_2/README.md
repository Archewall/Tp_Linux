# TP 2 Appréhension d'un système Linux
## I. Fichiers 
### 1. Find me 

j'utilise la commande 
```
pwd
```
qui va me donner le chemin qui me permet de voir comment me rendre a mon dossier utilisateur !
```
/home/alexandre
```
Pour trouver les logs SSH, je me suis rendu a la racine puis 
```
/var/log
```
le fichier qui m'interesse s'appel secure !

Enfin le dossier dédié à stocker tous les fichiers de configuration ce trouve dans :
```
/etc/ssh
```
le dossier qui m'interesse s'appel: sshd_config !

## II. Users
### 2. Nouveau user

```
sudo useradd -m -d /home/papier_alu/ -s /bin/bash marmotte
```
il faut à présent changer le mot de passe de marmotte pour pouvoir ce log
```
 sudo passwd marmotte
 ```
 avec comme mot de passe chocolat 

 ### 2. Infos enregistrées par le système

 ```
 [alexandre@localhost etc]$ cat passwd | grep marmotte

 ce qui donne : marmotte:x:1001:1001::/home/papier_alu/:/bin/bash
 ```

Pour déterminer le hash  de password de marmotte il me faut me connecter sous mon utilisateur puisque mamortte n'est pas dans le fichier sudoer
et pour me permttre de trouver le hash j'ulitile les commande :
```
[alexandre@localhost etc]$ sudo cat shadow

[alexandre@localhost etc]$ sudo cat shadow | grep marmotte
marmotte:$6$Rtc28SZ/bEMlgZFn$rseYPx1dNibhHClvdOzXcPW7mdthqHe6DCOF18H8ja9BvQyoYzCJxSqIps5ETL1Pn6JTlYVT9w7dTqfew9/XS1:19745:0:99999:7:::
```
### 3. Connexion sur le nouvel utilisateur

```
[alexandre@localhost etc]$ exit
logout
Connection to 10.2.1.2 closed.
```

```
PS C:\Users\Makhov> ssh marmotte@10.2.1.2
marmotte@10.2.1.2's password:
Last login: Tue Jan 23 14:51:10 2024 from 10.2.1.1
[marmotte@localhost ~]$
```

```
[marmotte@localhost home]$ cd alexandre
-bash: cd: alexandre: Permission denied
```


# Partie 2 : Programmes et paquets

## II. Programmes et processus

```
[alexandre@localhost ~]$ sleep 1000

sur un autre terminal :

[alexandre@localhost ~]$ ps -e | grep sleep
   1757 pts/0    00:00:00 sleep
```

Ensuite pour stopper le sleep j'utilise la commande :
```
[alexandre@localhost ~]$ kill  1757


[alexandre@localhost ~]$ sleep 1000
Terminated
```

### 2. Tâche de fond

```
[alexandre@localhost ~]$ sleep 1000 &
[1] 1792


[alexandre@localhost ~]$ ps -e | grep sleep
   1792 pts/0    00:00:00 sleep
```

### 3. Find paths 

pour trouver le chemin de sleep j'utilise la commande :

```
[alexandre@localhost /]$sudo find -name sleep
```
ensuite pour trouver le stockage !
```
[alexandre@localhost /]$ sudo ls -al /usr/bin/sleep | grep sleep
-rwxr-xr-x. 1 root root 36312 Apr 24  2023 /usr/bin/sleep
```

les différen chemin qui s'appel ".bashrc" :
```
[alexandre@localhost /]$ sudo find -name .bashrc
./etc/skel/.bashrc
./root/.bashrc
./home/alexandre/.bashrc
./home/papier_alu/.bashrc
```

Pour verifier que les programmes sont bien sotockés dans les dossier listé par PATH :

```
[alexandre@localhost /]$  echo $PATH
/home/alexandre/.local/bin:/home/alexandre/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin

- Sleep :
[alexandre@localhost /]$ which sleep
/usr/bin/sleep

- SSH :
[alexandre@localhost /]$ which ssh
/usr/bin/ssh
[1]+  Done                    sleep 1000  (wd: ~)
(wd now: /)

- PING :
[alexandre@localhost /]$ which ping
/usr/bin/ping
```

## III. Paquets 
install git 
```
[alexandre@localhost /]$ sudo dnf install git
```

trouver git :
```
[alexandre@localhost /]$ which git
/usr/bin/git
```
install nginx
```
[alexandre@localhost /]$ sudo dnf install nginx
```

chemin vers NGINX :
```
[alexandre@localhost nginx]$ pwd
/var/log/nginx

et le chemin vers dossier config

[alexandre@localhost nginx]$ pwd
/etc/nginx
```
Dossier qui contient la liste des URLs
```
[alexandre@localhost yum.repos.d]$ pwd
/etc/yum.repos.d
```

# Partie 3 : Poupée russe

récupérer le fichier meow
```
[alexandre@localhost ~]$ sudo dnf install wget -y

[alexandre@localhost ~]$ sudo wget https://gitlab.com/it4lik/b1-linux-2023/-/raw/master/tp/2/meow?inline=false

[alexandre@localhost ~]$ mv 'meow?inline=false' meow

[alexandre@localhost ~]$ ls
meow
```
Trouver dossier DAWA

```
[alexandre@localhost ~]$ file meow
meow: Zip archive data, at least v2.0 to extract
[alexandre@localhost ~]$ mv meow meow.zip
[alexandre@localhost ~]$ sudo dnf install unzip
[alexandre@localhost ~]$ sudo unzip meow.zip
Archive:  meow.zip
  inflating: meow

# XZ
[alexandre@localhost ~]$ file meow
meow: XZ compressed data
[alexandre@localhost ~]$ mv meow meow.xz
[alexandre@localhost ~]$ sudo unxz meow.xz

# BZIP2
[alexandre@localhost ~]$ file meow
meow: bzip2 compressed data, block size = 900k
[alexandre@localhost ~]$ sudo dnf install bzip2
[alexandre@localhost ~]$ mv meow meow.bz2
[alexandre@localhost ~]$ bzip2 -d meow.bz2

# RAR
[alexandre@localhost ~]$ file meow
meow: RAR archive data, v5
[alexandre@localhost ~]$ sudo mv meow meow.rar
[alexandre@localhost ~]$ sudo dnf install https://download1.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-8.noarch.rpm
[alexandre@localhost ~]$ sudo dnf install unrar
[alexandre@localhost ~]$ sudo unrar e meow.rar

# GZIP
[alexandre@localhost ~]$ file meow
meow: gzip compressed data, from Unix, original size modulo 2^32 145049600
[alexandre@localhost ~]$ mv meow meow.gz
[alexandre@localhost ~]$ sudo gunzip meow.gz

# TAR
[alexandre@localhost ~]$ file meow
meow: POSIX tar archive (GNU)
[alexandre@localhost ~]$ mv meow meow.tar
[alexandre@localhost ~]$ tar -xf meow.tar
```

Déterminer le chemin vers 

- 15 mo : 
```
[alexandre@localhost dawa]$ find -size 15M
./folder31/19/file39
```

- only 7 : 
```
[alexandre@localhost dawa]$ grep "777777" -r
folder43/38/file41:77777777777
```

- name cookie :
```
[alexandre@localhost dawa]$ find -name cookie
./folder14/25/cookie
```

- le seul fichier caché :
```
[alexandre@localhost dawa]$ find -type f -name ".*"
./folder32/14/.hidden_file
```

- fichier datant de 2014 :
```
[alexandre@localhost dawa]$ find -type f -newermt 2014-01-01 ! -newermt 2015-01-01
./folder36/40/file43
```

- fichier avec 5 dossiers parents :
```
[alexandre@localhost dawa]$ find -path "*/*/*/*/*/*/*"
./folder37/45/23/43/54/file43
```


