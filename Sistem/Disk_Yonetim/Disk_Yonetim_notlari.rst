=================
Disk yonetim komutlari
=================

:date: Cum 20 Şub 2015 13:54:54 EET
:comments: true
:categories: 
:tags: 
:Author: Orkun Gunay

Disk Baglama
============

#. Diski bagli oldugu partition'dan cikartip /var altindaki
icerikle yeni bir diske tasima;

    #. disk'te partition olusturulur

    ..code:: sh 

    parted --list
    parted /dev/sdX
    mklabel gpt
    mkpart primary 0% 100%
    quit

#. Partition ext4 olarak bicimlendirilir.

    ..code:: sh 

    mkfs.ext4 /dev/sdX1

#. Partition acilista sunucuya baglanacak sekilde eklenir.

ls -alh /dev/disk/by-uuid
vim /etc/fstab
UUID= ...       /var    ext4    0 2
```

* /var altina yazan calisan servisler durdurulur.
```
/etc/init.d/apache2 stop
/etc/init.d/atd stop
/etc/init.d/exim4 stop
/etc/init.d/cron stop
/etc/init.d/nginx stop
/etc/init.d/rsyslog stop
/etc/init.d/pure-ftpd stop
```

* /var altindaki icerik yedeklenir.
```
mv /var /var.old
```
* Yedeklenen icerik yeni olusturulan diske aktarilir.
```
mount /dev/sdX1 /mnt
cp -arp /var.old/ /mnt/ (data buyukse rsync kullanilabilir)
```
```
* disk /mnt'den umount edilir.
```
umount /mnt
```

* bos /var dizini olusturulur.
```
mkdir /var
```

* Yeni disk /var altina mount edilir.
``` 
mount /dev/sdX1 /var
```

* servisler baslatilir
```
/etc/init.d/apache2 start
/etc/init.d/atd start
/etc/init.d/exim4 start
/etc/init.d/cron start
/etc/init.d/nginx start
/etc/init.d/rsyslog start
/etc/init.d/pure-ftpd start
```

* Dizinde bulunan alt dizinleri oz yinelemeli buyukten kucuge siralama
```
du -s * | sort -nr | head -10
```

* partition table'i /dev/sda'dan /dev/sdb'ye kopylama
```
sfdisk -d /dev/sda | sfdisk /dev/sdb
```

* Disk doldugunda yapilacaklar (BSD)
ev dizini olarak 3 ayri yer mevcut, her ucune de bakip gereksiz dosyalar (eski yedek dosyalari vd.) silinir.  
```
/usr/local -  /home/ - /root
/usr/local/www
/usr/local/share/doc
/usr/local/vpopmail/
/usr/local/vpopmail/doc
```

* Sanal sunucuya  disk eklendiginde scsi bus'ları rescan ettirmek;
```
for i in /sys/class/scsi_host/*; do echo "- - -" > $i/scan; done
```

* Debian makinaya sd card mount etme;
```
blkid ile /dev altinda goruluyor mu bakilir;
```
/dev/mmc gibi bir yere baglaniyor, buradan mount edilebilir.

* Diski yeniden boyutlandirmak
```
e2fsck -f /dev/sdxx
resize2fs /dev/sdxx
```

* diske random 1gb dosya yazma
```
dd if=/dev/zero of=/<disk_yolu>/<dosya_adi> count=1000 bs=1M
```
* dd ile dosya yazarken yazma istatistiklerini gorme;
```
kill -USR1 <process_id>
```

* Android cihaz baglama;

    ..code:: sh 

    jmtpfs ~/mtp

* umount etmek icin;

    ..code:: sh 

    fusermount -u ~/mtp

`Kaynak:Archwiki<https://wiki.archlinux.org/index.php/MTP>`_

#. bagli diskleri gorme;

    ..code:: sh 

    cat /proc/mounts
    
    cat /proc/self/mounts

