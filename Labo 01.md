# Labo 01

## Task 1: Prepare the backup disk

1. ![](/home/jerome/HEIG/Labo/AIT/Labo01/img/mount.png)

![](/home/jerome/HEIG/Labo/AIT/Labo01/img/mount_1.png)

3. ```sh
   parted 
   mktable
   > msdos
   mkpart primary 0% 50%
   mkpart primary 50% 100%
   quit
   
   mkfs.vfat /dev/sdb1
   mkfs.ext4 /dev/sdb2
   
   mount /dev/sdb1 /mnt/backup1
   mount /dev/sdb2/mnt/backup2
   ```

6. ![](/home/jerome/HEIG/Labo/AIT/Labo01/img/df.png)

## Task 2: Perform backups using tar and zip

```sh
 tar -cvpzf backup.tar.gz --exclude=/backup.tar.gz --exclude=/home/jerome/Téléchargements /home/jerome
```

> Note: we used gz cuse its the most used

```sh
  tar -tvf backup.tar.gz
```

> Pour lister

```sh
cd tmp; tar -zxvf ../backup.tar.gz 
```

> restore of archive 