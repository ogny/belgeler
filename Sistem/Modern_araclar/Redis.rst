============
Redis 2.8.19
============

Kurulum
-------

* Epel repo ekle degilse::

    rpm --import https://fedoraproject.org/static/0608B895.txt
    rpm --import http://rpms.famillecollet.com/RPM-GPG-KEY-remi
    curl -O http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    curl -O http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
    rpm -Uvh remi-release-6*.rpm epel-release-6*.rpm

    vi  /etc/yum.repos.d/remi.repo
    enabled=1

    yum install -y redis


Yapilandirma
============

Sistem Genel
------------

* Acilista baslat::
  
    chkconfig --level 345 redis on

* hang olmasin::

    echo "vm.overcommit_memory=1" | tee -a /etc/sysctl.conf && sysctl -p

* memory kullanimini denetleme ve gecikmelere karsi kernel'de kapat::

    echo never > /sys/kernel/mm/transparent_hugepage/enabled

* sistemde swap olustur ve maxmemory'i sinirlandir.
  degisiklikten sonra redis'i restart et::
    ulimit -m <deger> 
    max user processes value = pending signals value
    /etc/security/limits.d/90-nproc.conf
    * soft nproc 64000

    /etc/security/limits.conf
    * -       nofile          64000

Not: It is not recommend to set the "hard" limit for nofile for the oracle user
equal to /proc/sys/fs/file-max. If you do that and the user uses up all the
file handles, then the entire system will run out of file handles. This may
prevent users logging in as the system cannot open any PAM modules that are
required for the login process. That is why the hard limit should be set to
63536 and not 65536.

`Kaynak:redhat
<https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Tuning_and_Optimizing_Red_Hat_Enterprise_Linux_for_Oracle_9i_and_10g_Databases/chap-Oracle_9i_and_10g_Tuning_Guide-Setting_Shell_Limits_for_the_Oracle_User.html>`_

* incelenecek::

    - Controls the maximum number of shared memory segments, in pages
    kernel.shmall = 4294967296
    fs.file-max = 200000


Redis Yapilandirma
------------------

(calisiliyor)


Sentinel Yapilandirma
---------------------

* Config dosyasi bulundurmak sart, ornek conf redis ile beraber geliyor.

Sentinel'ler dagitik bir sekilde redis master'i izliyor, coktugune karar
vermeleri uc asamali;

    - Master'dan cevap alamadiginda **subjectively down** (also known as SDOWN)
    - Down olsa bile yeni master'i atamak icin sentinel yeterli cogunlugunun
      (quorum) onayina ihtiyac var. (ODOWN)
    - Quorum saglandiginda, kalan sentinel sayisi kadar sentinel'e authorize
      olmasi gerekiyor.

#. `parallel-sync` ile ayni anda kac slave'in master'la sync olacagini
   belirliyorsun. bir tanesi sync olurken digerlerini eski data'dan yanit
   verecek sekilde duzenlemek avantaj saglar.

#. Epoch Yapilandirmasi

#. Because every configuration has a different version number, the greater
   version always wins over smaller versions.

#. sentinel'leri karsilikli olarak yapilandirmaya ihtiyac yok, ayni master'i
   dinleyen sentinel'ler birbirlerini buluyor.

#. Partition kavrami incelenecek : eski master'i isole etmede kullaniliyor.
   (network Partition olarak geciyor.)

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

    - Sentinel cikartmak.
    - old master'i veya ulasilamayan slave'leri cikartmak.

^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Yapilandirma icin gerekenler
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

virtual-ip (haproxy)

Sanal makinalar
--------------
- test-ha1
- test-ha2
- test-redis1 (sentinel1)
- test-redis2 (sentinel2)
- sentinel3

haproxy
~~~~~~~

* logging::

    cat << EOF > /etc/rsyslog.d/49-haproxy.conf
    local2.* -/var/log/haproxy.log
    & ~
    EOF

    vi /etc/rsyslog.d/49-haproxy.conf
    $ModLoad imudp
    $UDPServerRun 514
    $UDPServerAddress 127.0.0.1
    
    /etc/init.d/rsyslog restart

* Kurulum - Yapilandirma::

    yum install -y haproxy keepalived
    echo "net.ipv4.ip_nonlocal_bind=1" | tee -a /etc/sysctl.conf && sysctl -p
    
    mv /etc/keepalived/keepalived.conf{,.org}
    mv /etc/haproxy/haproxy.cfg{,.org}
    
    vi /etc/haproxy/haproxy.cfg

defaults'ta degistirilenler::
      mode              tcp
      timeout connect   2s
      timeout  client   120s 
      timeout  server   120s 
      maxconn           4096
      frontend  redis
      bind   :6379
      default_backend redis_backend
      backend redis_backend
      option tcp-check
haproxy will look for the following strings to determine the master::
      tcp-check send PING\r\n
      tcp-check expect string +PONG
      tcp-check send info\ replication\r\n
      tcp-check expect string role:master
      tcp-check send QUIT\r\n
      tcp-check expect string +OK
these are the ip’s of the two redis nodes::
      server redis1 <redis_ip>:6379  check inter 1s
      server redis2 <redis_ip>:6379  check inter 1s
    
* Servis baslatilir::
      /etc/init.d/haproxy start
