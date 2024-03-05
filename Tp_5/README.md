# TP 5 : We do a little scripting

## partie 1 :

je lance le programme avec :

./idcard.sh

```bash
Machine name: SFAqua

OS: Rocky Linux and kernel version is : 5.14.0-284.11.1.el9_2.x86_64

ip : 10.2.1.12/24

RAM : 336Mi memory available on 771Mi Total memory 

Disk 4.9G space left

Top 5 processes by RAM usage : 
- node
- node
- node
- python3
- NetworkManager

Listening ports :
      - 323 udp : chronyd
      -   : chronyd
      - 34155 tcp : sk:3
      - 22 tcp : sshd
      -   : sshd

PATH directories :
/home/alexandre/.vscode-server/bin/903b1e9d8990623e3d7da1df3d33db3e42d80eda/bin/remote-cli
/home/alexandre/.local/bin
/home/alexandre/bin
/usr/local/bin
/usr/bin
/usr/local/sbin
/usr/sbin

Here is your random cat : https://cdn2.thecatapi.com/images/dv7.jpg
```

## Partie 2:

je lance le programme avec :  

./yt.sh
```bash
#! /bin/bash
#SF Aqua
#04/03/2024

TITLE=$(youtube-dl -e "$1")
echo $TITLE
if [ ! -d "/srv/yt/downloads/$TITLE" ]; then
    mkdir "/srv/yt/downloads/$TITLE"
fi

youtube-dl -o "/srv/yt/downloads/$TITLE/$TITLE.mp4" --format mp4 "$1" 

```