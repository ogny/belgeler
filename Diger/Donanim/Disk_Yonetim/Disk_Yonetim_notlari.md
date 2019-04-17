Disk yonetim komutlari
=================

:date: Cum 20 Şub 2015 13:54:54 EET
:comments: true
:categories: 
:tags: 
:Author: Orkun Gunay

* partition'lar hakkinda bilgi::
    lsblk -o name,mountpoint,label,size,uuid


Disk Baglama
============

#. loop device baglama (mountpoint kontrolu yapmak icin)
  ...code:: sh

  dd if=/dev/sda1 of=dummydevice.img 
  losetup /dev/loop0 dummydevice.img
  mkdir <dizin>
  mount -o loop dummydevice.img <dizin>


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
    mke2fs -t ext4  /dev/sdX1

#. Partition acilista sunucuya baglanacak sekilde eklenir.

```
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
du --max-depth=1 /tmp/\* | sort -r -k1,1n
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
for i in /sys/class/scsi_host/\*; do echo "- - -" > $i/scan; done
```

* Debian makinaya sd card mount etme;
```
blkid ile /dev altinda goruluyor mu bakilir;
```
/dev/mmc gibi bir yere baglaniyor, buradan mount edilebilir.
```

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
    apt-get install mtp-tools jmtpfs
    mkdir ~/mtp ; chmod 777 ~/mtp
    jmtpfs ~/mtp

* umount etmek icin;

    ..code:: sh 

    fusermount -u ~/mtp

`Kaynak:Archwiki<https://wiki.archlinux.org/index.php/MTP>`_
`Kaynak:Debian Wiki<https://wiki.debian.org/mtp>`_

#. bagli diskleri gorme;

    ..code:: sh 

    cat /proc/mounts
    
    cat /proc/self/mounts

#. Diskleri göremediği durumda (partition table bozuksa)::

   Ubuntu'da fixparts kullanilabilir.

   http://superuser.com/questions/744916/ubuntu-14-04-installer-doesnt-show-existing-partitions

#. Disk I/O ve load average   
`Kaynak:blog<https://prutser.wordpress.com/2012/05/05/understanding-linux-load-average-part-2/>`_

#. Promise SAN Lun

* LUN; genel olarak bir storage ünitesi üzerinde bir ya da daha fazla sunucunun kullanımına sunulan disk alanına denir. Storage üniteleri üzerinde takılan diskler ile raid vs. işlemler tamamlandıktan sonra ortaya çıkan kullanılabilir disk alanı üzerinde LUN’lar oluşturulur. Daha sonra bu LUN bir (ya da birkaç) sunucuya atanarak kullanılır. Sunucu atanan LUN’u kendi local disklerini formatlar gibi formatlar ve üzerine data yazıp okur. Kısaca sunucu kendi lokalindeki bir storage ünitesini değil de farklı bir lokasyondaki (SAN-NAS-DAS) storage ünitesini kullanıyorsa bu birim LUN olarak adlandırılır.Aşağıdaki örnek daha açıklyıcı olacaktır.

#. Linux'ta Windows ve Mac OS Uyumlu USB biçimlendirme:
   Ortam: Debian 8.5 Jessie
   Gereksinimler:
   sudo apt install -y exfat-fuse exfat-utils
   Yapılandırma:
   sudo parted /dev/sdb
   mkpart primary 0% 100%
   set 1 msftdata
   sudo mkfs.exfat -n <görünecek_isim> /dev/sdb1
