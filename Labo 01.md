# Labo 01

## Task 1: Prepare the backup disk

1. List all the files called `/dev/hd*` & `/dev/sd*`

We do not have any disks mounted as `hd*` or `sd*`. This is simply because, we have a SSD and it is mounted as `nvme*`. If we list the files called `/dev/nvme*` we can see our hard drive and all of it's partitions.

![](./img/withUsb.png)

As we can see with `mount`, only the `nvme0n1p7`, `nvme0n1p1` & `nvme0n1p4`.

> Note: `/` isn't mounted as a `nvme*` disk because it's encrypted with `luks` so it's visible as a `luks` disk.

![](img/mount.png)

2. Attach the disk to your computer

When plugging the a new disk to our computer, we can see that there is a `/dev/sd*` disk mounted which only has 1 partition.

![](img/withUsb.png)

3. Create a partition table on the disk and create two partitions of equal size using the `parted` tool.

```sh
parted 
(parted) print # see capture 1
(parted) mktable
(parted) > msdos
(parted) print free # see capture 2
(parted) mkpart primary 0% 50%
(parted) mkpart primary 50% 100%
(parted) quit

mkfs.vfat /dev/sdb1
mkfs.ext4 /dev/sdb2

mount /dev/sdb1 /mnt/backup1
mount /dev/sdb2/mnt/backup2
```

![](img/parted_print_1.png)

> Note: Print before mktable

![](img/parted_print_2.png)

> Note: Print after mktable

We checking the amount of free space on these file systems, we can see that they are pretty much empty. Which is normal since we just created them.

![](./img/df.png)

## Task 2: Perform backups using tar and zip

```sh
 tar -cvpzf backup.tar.gz --exclude=/backup.tar.gz  /home/jerome
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

```
gunzip backup.tar.gz; find /home/jerome/HEIG/A1 -mtime +1454 -exec tar -uvf backup.tar  {} \; ; gzip  backup.tar
```

> en exécutant la commande à la date du 17 septembre

## Task 3: Backup of file metadata

![](./img/beforetar.png)

![](./img/aftertar.png)

> By default the **Tar** command overwrite metadata when extracting. With the option **--no-overwrite-dir** we can only preserve the last modification time. 

## Task 4: Symbolic and hard links

```sh
tar -h --hard-dereference -cvpzf backup.tar.gz test
```

> the **-h** archive and dump the files they point to for symlink and the **--hard-dereference** archive and dump the files they point to for hardlink 