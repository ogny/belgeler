XFS
===

#. olusturma-mount etmede ext4'ten farki yok
#. mount ederken dikkat: If you have environment with filesystem above 2 TB ,
   you could try benchmark with mounting with inode64 option.
# mount -o inode64 /dev/device /mount/point

Genisletme
----------

yeni bir partiton olustur.

#. Next all you have to do is run the xfs_growfs command with the -d switch (to
   grow the data part of the file system) and the filesystem will be grown to
   the new size of the partition.

.. code:: sh

xfs_growfs -d /mnt/db

`Keynak:linoxide <http://linoxide.com/file-system/create-mount-extend-xfs-filesystem/`

Case'ler
--------
#. Dosyalari goruntulemek istediginde olusan hata: "Structure needs cleaning"
   fs'i dizinden umount et. mount edemezsen (file system busy now)
    umount -f -l /dizin

NFS servisini durdur
    service nfs stop

xfs_repair'le check et, sorun yoksa repair et::
    xfs_repair -n -vv /dev/<disk_yolu>
    xfs_repair -vv /dev/<disk_yolu>

Phase 2 - using internal log
        - zero log...
zero_log: head block 895671 tail block 895667
ERROR: The filesystem has valuable metadata changes in a log which needs to
be replayed.  Mount the filesystem to replay the log, and unmount it before
re-running xfs_repair.  If you are unable to mount the filesystem, then use
the -L option to destroy the log and attempt a repair.
Note that destroying the log may cause corruption -- please attempt a mount
of the filesystem before doing this.


# mkfs.xfs -f -L /data -d agcount=50 -l size=1024m,version=2 /dev/sdb1
