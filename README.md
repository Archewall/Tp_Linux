# TP1 casser avant de construire

## II. CASSER

### 2. FICHIER

Pour rendre ma machine inutilisable je me suis rendu dans :

```bash
cd /
cd boot
rm  vmlinuz-0-rescue-444f7b0407714d8fa7ac6619538e8755
rm  vmlinuz-5.14.0-284.11.1.el9_2.x86_64
```

ce qui m'empêche de démarrer ma vm.

### 3. Utilisateurs

Pour lister tout les utilisateurs je me suis rendu dans :

```bash
cd /etc/shadow
```

ensuite j'ai nano dans le fichier "shadow" pour voir tout les utilisateurs et j'ai tout supprimé.

### 4. Disques

Pour remplir mon disque dur j'ai éssayé 2 méthode.
d'abord je me suis rendu dans le dev et j'ai regarder quel disque est le plus utilisé

2 disque sont très utilisé :
/dev/sda1 utlisé a 22%
/dev/mapper/rl-root 20%

```bash
[root@localhost /]# cd dev
[root@localhost dev]# df -h
[root@localhost dev]# dd if=/dev/0 of=/dev/sda1
[root@localhost dev]# dd if=/dev/zero of=/dev/mapper/rl-root
```

### 5. Malware

