=================
disk genisletme
=================

:date: Çrş 04 Mar 2015 19:07:58 EET
:comments: true
:categories: 
:tags: 
:Author: Orkun Gunay

diski umount et
partition table'ini parted'la silip yeniden olustur
e2fsck -f -p /dev/sdb1 
resize2fs -p /dev/sdb1 <yeni_boyut>
