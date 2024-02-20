# Partie 1 : Partitionnement du serveur de stockage

### Partitionner le disque à l'aide de LVM

. Créer 2 physical volumes (PV) :

```bash
[alexandre@Storage ~]$ sudo pvcreate /dev/sdb
[sudo] password for alexandre:
  Physical volume "/dev/sdb" successfully created.

[alexandre@Storage ~]$ sudo pvcreate /dev/sdc
 Physical volume "/dev/sdc" successfully created.

  sudo pvs

 PV         VG Fmt  Attr PSize PFree
  /dev/sdb      lvm2 ---  2.00g 2.00g
  /dev/sdc      lvm2 ---  2.00g 2.00g

```

. Créer un nouveau volume group (VG)

```bash
[alexandre@Storage ~]$  sudo vgcreate storage /dev/sdb
  Volume group "storage" successfully created

 [alexandre@Storage ~]$ sudo vgs
  VG      #PV #LV #SN Attr   VSize  VFree
  storage   1   0   0 wz--n- <2.00g <2.00g

  [alexandre@Storage ~]$ sudo vgextend storage /dev/sdc
[sudo] password for alexandre:
  Volume group "storage" successfully extended

[alexandre@Storage ~]$ sudo vgs
    VG      #PV #LV #SN Attr   VSize VFree
  storage   2   0   0 wz--n- 3.99g 3.99g
```


.Créer un nouveau logical volume (LV)
```bash
[alexandre@Storage ~]$ sudo lvcreate -l 100%FREE storage -n last_storage
[sudo] password for alexandre:
  Logical volume "last_storage" created.

[alexandre@Storage ~]$ sudo lvs
  LV           VG      Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  last_storage storage -wi-a----- 3.99g
```

### Formater la partition

. Formater la partition en ext4
```bash
[alexandre@Storage ~]$ sudo mkfs -t ext4 /dev/storage/last_storage
```

### Monter la partition

. montage de la partition
```bash
[alexandre@Storage ~]$ sudo mkdir /storage

[alexandre@Storage ~]$ sudo mount /dev/storage/last_storage /storage

[alexandre@Storage ~]$ df -h| grep /storage
/dev/mapper/storage-last_storage  3.9G   24K  3.7G   1% /storage
```

prouver que je peux bien lire et écrire dans ma partition :
```bash

[alexandre@Storage storage]$ sudo touch maman

[alexandre@Storage storage]$ sudo !!
sudo nano maman

[alexandre@Storage storage]$ sudo cat maman
peppa
```
