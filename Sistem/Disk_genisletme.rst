=================
disk genisletme
=================

:date: Çrş 04 Mar 2015 19:07:58 EET
:comments: true
:categories: 
:tags: 
:Author: Orkun Gunay

ext4
----

diski umount et
partition table'ini parted'la silip yeniden olustur
e2fsck -f -p /dev/sdb1 
resize2fs -p /dev/sdb1 <yeni_boyut>

lvm2
----

* makaledeki gibi fdisk'le yeni bir partition olusturulup lvm2 hex code'u verilir

http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1006371

* VolGroup00 yerine VolGroup-lv_root yaz.
* vgdisplay VolGroup | grep "Free" ile kalan bos yeri bul.
* lvextend -L+<Bos_yer>G /dev/VolGroup/lv_root
* resize2fs -p /dev/VolGroup/lv_root



