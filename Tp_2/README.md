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