#############################################################################
#
# Beyaz Bilgisayar
# Orkun Gunay
#
#############################################################################
#### Paket Yonetimi
* paketlerin versiyon bilgisini almak için: pkg_version
* paketlerin sürümünü görmek için, ornek:
/usr/local/sbin/httpd -v

#### Sistem
* portları güncellemek için; her seferinde önce diff'leri tutup sonra update
* ediyorsun, daha sonra da güvenli olması için bu yöntemi tekrar edebilirsin.
portsnap fetch
portsnap update
cd /usr/ports
pkg_info # tüm kurulu paketler
pkg_check # yükseltilecek paketleri buluyoruz.
pkg_delete -f paket_tam_sürüm
pkg_add -r paket_adı 
* base'i update etmek için svn kullan
binary update: freebsd -update

* Binary bulamadigin paketi derleyerek kurmak icin
make fetch
make config
vim Makefile
make make install clean

#### Sql
/var/db/mysql
/usr/local/bin/mysqld

* mysqlcheck hatasının çözümü için;
* Mysqlin log'larından hangi tabloda hata olduğunu bulduk.
/var/db/mysql/cache
select count(*) from tablo_adı;
delete tablo_adı;

* Log'lari human-readable yapan uygulama: tai64nlocal

#### Qmail
* Qmail çalışırken fix eden uygulamaları çalıştırırsan (qfixq gibi) kuyruk bozuluyor.
* Sorunlari bulma ve cozme;
* Once servislerin tail ile loglarina bakiyoruz. Log dizinleri;
/var/log/
/var/qmail/service/qmail/log 

* Sorunlari yakalayamiyorsak servisleri durdurup queue'ya mudahale edelim.
/usr/local/etc/rc.d/svscan.sh stop
/usr/local/etc/rc.d/dovecot stop
/usr/local/etc/rc.d/mysql-server stop 
/usr/local/etc/rc.d/apache2x stop

qmqtool -s # kuyrugun ozet bilgisi
qmqtool -l # kuyruktaki tum mailleri doker.

* qmqtool olmayan sunucularda;
qmail-qstat rapor
qmail-qread tek tek kuyruktaki mailler hakkinda bilgi verir.

* En iyi hata bulma yontemi;
qmqtool -l |less #kuyruktaki maillere bakarsin, gonderen kisminda 

* Gercek bir hesap adresi yoksa o mailin id'sini alirsin.
qmqtool -v mail_id # ile hangi gercek hesaptan gonderildigini tespit edersin.

* Cracklenen kullanicinin parolasini sifirlarsin.
/usr/local/vpopmail/bin/vpasswd cracklenen_hesap

* Cracklenen adresten gonderilen mailleri sil.
qmqtool -d -f "cracklenen_hesap" 

* Cok sayida hesabin cracklendigini tespit ettiysek alternatif olarak 24 saatten fazla suredir bekleyen mailleri sil.
qmqtool -d -o 24                

* Fazlaysa bu sekilde temizlemeyi onermiyoruz, asagidaki yontemi uyguluyoruz;
cd /var/qmail/ 
mv queue queue_yeni_ad

* Daha once hazirladigimiz, bu dizinde bulunan queue.tar.gz dosyasini aciyoruz. (Dosya yoksa scp ile sunucuya gonderiyoruz.)
scp queue.tar.gz -P PortNo KullanıcıAdı@IPAdresi:~/uzak/sunucudaki/dizinin/yolu .
tar -zxvf queue.tar.gz

* Qmail servislerinin tamamen durup durmadigini kontrol ediyoruz
ps aux |grep mail

* Artik queue'i queue olarak adini degistirip servisleri baslatabiliriz.
mv queue. queue
/usr/local/etc/rc.d/svscan.sh start
/usr/local/etc/rc.d/dovecot start
/usr/local/etc/rc.d/mysql-server start
/usr/local/etc/rc.d/apache2x start

* /var/qmail/control'daki değişiklikler
* Keepalive kuyrukta kalma süresini düşürebilirsin --> 86400.

* Cracklenen kullaniciya gelen spam iletilerin yedeklenmesi
cd /usr/local/vpopmail/domains/Alan_Adi/Cracklenen_Hesap/cur
mkdir ../Cracklenen_Hesap/mailleri
find * -type f -name '*' -exec mv {} $../Cracklenen_Hesap/mailleri/. \;

* current log'da (/var/qmail/log/qmail/current)  saniyede bir asagidaki uyariyi basiyorsa:
alert: cannot start: qmail-send is already running
svscan zaten calisan qmail-send'i baslatmaya calisiyor, 
Cozum: calisan qmail-send'i oldurup svscan'i yeniden baslatiyoruz.
kill -ALRM `ps ux | grep qmail-remote | grep -v grep | awk '{print $1}'`
kill -ALRM `ps ax | grep qmail-send | grep -v grep | awk '{print $1}'`
process'in olup olmedigini elle kontrol et; ps aux |grep qmail-send
process olmediyse kill -9 ile oldur

#### Arastirilacak;
* Giden mailler fortimail'den gecerken fortimail-qmail auth etme.
* Dovecot'ta userquota denetimi nasil yapabiliyoruz.
* Spam throttling sunucu en fazla ne kadar mail gonderebilir?

#### Kullanici Yonetimi
* Kullanicinin e-posta hesabina alias tanimlayabiliyoruz;
cd /usr/local/vpopmail/bin
./valias -i gercek_hesap_adi alias_hesap_adi
Alias'i silme parametresi -d

* Yeni bir e-posta domaini eklemek:
./vadddomain korfeztatil.com.tr postmaster_parolasi
ilk acilista postmaster kullanicisini kendi yaratiyor.

* Yeni bir e-posta kullanicisi eklemek:
/usr/local/vpopmail/bin/vadduser kullanici_adi@domain

* Ek dosyalarin alabilecegi en buyuk boyut, asagidaki dosya altinda belirtiliyor;
/var/qmail/control/databytes

* mail sunucu asagidaki hatayi verip mail atmayi reddediyorsa;
"DNS temporary failure (#4.3.0)"
Problem dns tarafinda degil, mail sunucunun resolv.conf'unda recursive dns sunucu olmadigindan istekleri cozemiyor.

* mail sunucuyu kara listeden cikartma;
kara listede olup olmadigini gormek icin;
http://multirbl.valli.org/lookup/IP.html
Sorbs'a girmisse http://www.sorbs.net/ adresinden cikartilir.

* Mail sunucu kara listede olmamasına rağmen kara liste uyarısı alınıyorsa;
Sunucu senderscore.org ve senderbase.org'de kontrol edilir.

* onde bir guvenlik cihazi oldugunda (ornegin fortimail) istekleri onun uzerinden gecirmek icin;
/var/qmail/control/smtproutes altina su formatta yaziyoruz
:IP_ADRESI, ornegin
:192.168.2.1

