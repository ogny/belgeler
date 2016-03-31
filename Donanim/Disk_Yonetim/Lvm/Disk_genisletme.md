disk genisletme
---

* parted ile disk'in label/flag/fstype'i belirlenir;

```
parted /dev/sdX
mklabel msdos
mkpart primary ext4 0% 100%
set 1 lvm on
quit
mkfs.ext4 /dev/sdX1
fdisk -l /dev/sdX1
```
* physical volume olusturulur, volume group'a eklenir
```
pvcreate /dev/sdX1
vgextend VolGroup /dev/sdX1
```

* Volume group'taki bos alan bulunup VG o kadar buyultulur, kontrol edilir
```
vgdisplay VolGroup | grep "Free"
lvextend --extents +100%FREE /dev/VolGroup/lv_root
resize2fs -p /dev/VolGroup/lv_root
df -h /
```

ext4
----

diski umount et
partition table'ini parted'la silip yeniden olustur
e2fsck -f -p /dev/sdb1 
resize2fs -p /dev/sdb1 <yeni_boyut>


Varolan bir partition'u lvm grubuna dahil etme;

lvm2
----

* makaledeki gibi fdisk'le yeni bir partition olusturulup lvm2 hex code'u verilir

http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1006371

* VolGroup00 yerine VolGroup-lv_root yaz.

#. bos alani hesaplama::

    vgdisplay VolGroup | grep "Free" 

#. Bos alani ekleme::

    lvextend -l 100%FREE /dev/VolGroup/lv_root

#. Partition eklenen diski genisletme::

    resize2fs -p /dev/VolGroup/lv_root

* lv_home'dan lv_root'a transfer (Centos default kurulumda 50G disindaki alani /home'a bagliyor)

http://serverfault.com/questions/524962/how-to-integrate-home-back-into-main-partition-then-grow-partition

* Yukaridaki son iki adim eklenir.


