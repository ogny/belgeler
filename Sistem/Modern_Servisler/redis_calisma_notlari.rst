Calisma Notlari
===============

Redis Persistence
-----------------



Sentinel Genel
--------------

* Config dosyasi bulundurmak sart, ornek conf redis ile beraber geliyor.

Sentinel'ler dagitik bir sekilde redis master'i izliyor, coktugune karar
vermeleri uc asamali;

    1. Master'dan cevap alamadiginda **subjectively down** (also known as SDOWN)
    2. Down olsa bile yeni master'i atamak icin sentinel yeterli cogunlugunun
       (quorum) onayina ihtiyac var. (ODOWN)
    3. Quorum saglandiginda, kalan sentinel sayisi kadar sentinel'e authorize
       olmasi gerekiyor.

#. `parallel-sync` ile ayni anda kac slave'in master'la sync olacagini
   belirliyorsun. bir tanesi sync olurken digerlerini eski data'dan yanit
   verecek sekilde duzenlemek avantaj saglar.

#. Epoch Yapilandirmasi

#. Because every configuration has a different version number, the greater
   version always wins over smaller versions.

#. sentinel'leri karsilikli olarak yapilandirmaya ihtiyac yok, ayni master'i
   dinleyen sentinel'ler birbirlerini buluyor.

#. Partition relational db'lerdeki sharding: eski master'i isole etmede, birden
   cok master icin kullaniliyor, caching yapisinda sorunsuz calisabilir,
   pratikte old master'dan slave'e donuste karsilasilan bir durum.

#. slave'den master'a geciste tum slave'lerin ayni run id'ye sahip olmasi
   oneriliyor, gecisin statik degil dinamik olmasi icin. 

#. bir redis instance'inin istemci dogrulamasi gerektirmeden, sadece slave
   mode'da calismasi icin;
   in the uncommon case where you need a slave that is accessible without
   authentication, you can still do it by setting up a slave priority of zero
   (that will not allow the slave to be promoted to master), and configuring
   only the masterauth directive for this slave, without the requirepass
   directive, so that data will be readable by unauthenticated clients.

#. Sentinel ekleme icin sadece aktif master'i monitor edecek sekilde
   yapilandirmak yeterli. 

#. Case by case anlatilan konular;

    #. Sentinel cikartmak.
    #. old master'i veya ulasilamayan slave'leri cikartmak.



Hatalar
~~~~~~~


Calisilacak
~~~~~~~~~~~~

* sistemde swap olustur ve maxmemory'i sinirlandir.
  degisiklikten sonra redis'i restart et::

    ulimit -m <deger> 
    max user processes value = pending signals value

Not: It is not recommend to set the "hard" limit for nofile for the oracle user
equal to /proc/sys/fs/file-max. If you do that and the user uses up all the
file handles, then the entire system will run out of file handles. This may
prevent users logging in as the system cannot open any PAM modules that are
required for the login process. That is why the hard limit should be set to
63536 and not 65536.

* incelenecek::

    kernel.shmall = 4294967296
    fs.file-max = 200000

