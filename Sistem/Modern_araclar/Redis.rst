============
Redis 2.8.19
============

Sunucu Ortami
=============

virtual-ip (haproxy)

#. ha1-redis1-sentinel1

#. ha2-redis2-sentinel2

#. sentinel3

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

     yum install -y redis haproxy keepalived screen telnet vim-enhanced nfs-utils nfs-utils-lib


Yapilandirma
============

Sistem Genel
------------

* Acilista baslat::
  
    chkconfig redis on

* hang olmasin::

    echo "vm.overcommit_memory=1" | tee -a /etc/sysctl.conf && sysctl -p

* memory kullanimini denetleme ve gecikmelere karsi kernel'de kapat::

    echo never > /sys/kernel/mm/transparent_hugepage/enabled

* Kaynak kullanimini arttir::

    /etc/security/limits.d/90-nproc.conf
    * soft nproc 64000

    /etc/security/limits.conf
    * -       nofile          64000

* ``TCP backlog setting of 511 cannot be enforced because
  /proc/sys/net/core/somaxconn is set to the lower value of 128``::

   echo "net.core.somaxconn=65535"  | tee -a /etc/sysctl.conf && sysctl -p

* NFS erisimi saglanir::

    mkdir -p /media/sas2tb/ipam/redis/backups /media/sas600gb
    chkconfig nfs on
    service  rpcbind start
    service  nfs start
    mount -t nfs -rw <nfs_server_ip>:/media/sas2tb/ipam/redis/
    /media/sas2tb/ipam/redis/
#fstab'a eklenecek::

    <nfs_server_ip>:/media/sas600gb /media/sas600gb
    rw,sync,no_root_squash,no_subtree_check,noatime 0 0
    <nfs_server_ip>:/media/sas2tb/ipam/redis/ /media/sas2tb/ipam/redis/
    rw,sync,no_root_squash,no_subtree_check,noatime 0 0

Redis Yapilandirma
-------------------

::

    cp /etc/redis.conf{,.org}
    vim /etc/redis.conf


#. Master ve slave'de beraber default conf'ta degistirilecek::

    port 6380
    bind 0.0.0.0 

Redis Replication
------------------

* Sadece slave'e eklenecek::

    slaveof <master_ip> 6380

* Servis baslatilir::

    /etc/init.d/redis start

* Her iki sunucuda role'ler kontrol edilir::

    redis-cli -p 6380 info |grep ^role


Sentinel
~~~~~~~~

* Tum Sentinel sunucularda::

    chkconfig redis-sentinel on
    cp /etc/redis-sentinel.conf{,.org}
    vim /etc/redis-sentinel.conf
    sentinel monitor mymaster <master_ip> 6380 2
    /etc/init.d/redis-sentinel start

* standalone sunucuda::

    /etc/init.d/redis stop
    chkconfig redis off


haproxy
~~~~~~~

* logging::

    cat << EOF > /etc/rsyslog.d/49-haproxy.conf
    local2.* -/var/log/haproxy.log
    & ~
    EOF

    vim /etc/rsyslog.conf
    $ModLoad imudp
    $UDPServerRun 514
    $UDPServerAddress 127.0.0.1

    /etc/init.d/rsyslog restart

* Kurulum - Yapilandirma::

    chkconfig redis on

    mv /etc/haproxy/haproxy.cfg{,.org}
    vim /etc/haproxy/haproxy.cfg

::
    
    global
         log   127.0.0.1   local2  notice
         maxconn   4096
         chroot   /var/lib/haproxy
         user  nobody
         group  nobody
         daemon
    
    defaults
        log  global
        mode  tcp
        retries   3
        option  redispatch
        maxconn   2000
        timeout  connect   2s
        timeout  client   120s
        timeout  server   120s
    
    frontend  redis_master
        bind   :6379
        default_backend redis_backend
    
    backend redis_backend
    option tcp-check

#haproxy will look for the following strings to determine the master::

    tcp-check send PING\r\n
    ecp-check expect string +PONG
    tcp-check send info\ replication\r\n
    tcp-check expect string role:master
    tcp-check send QUIT\r\n
    tcp-check expect string +OK
#these are the ip’s of the two redis nodes::

    server redis1 <redis_ip>:6380  check inter 1s
    server redis2 <redis_ip>:6380  check inter 1s

* Servis baslatilir::

    /etc/init.d/haproxy start

Keepalived
~~~~~~~~~~

::
    echo "net.ipv4.ip_nonlocal_bind=1" | tee -a /etc/sysctl.conf && sysctl -p

    mv /etc/keepalived/keepalived.conf{,.org}
    vim /etc/keepalived/keepalived.conf

    vrrp_script chk_haproxy {
    script "killall -0 haproxy" # verify the pid existance
    interval 2 # check every 2 seconds
    weight 2 # add 2 points of prio if OK
    }
    
    vrrp_instance VI_1 {
            interface eth0 # interface to monitor
            state MASTER # other is BACKUP
            virtual_router_id 51 # Assign one ID for this route
            priority 101 # 101 on master, 100 on backup
            virtual_ipaddress {
            <Virtual_IP>
            }
            track_script {
            chk_haproxy
            }
#sadece master'da::

            notify_backup "/var/lib/redis/scripts/stop_redis.sh"
    }

    
#. Redis stop betigi::

   cat /media/sas2tb/ipam/redis/scripts/stop_redis.sh
   #!/bin/bash
   kill -15 `ps -ef |grep redis-server | grep -v grep  | awk '{print $2}'`

#. Redis backup betigi (sadece master'da calistirilacak)::

   cat /media/sas2tb/ipam/redis/scripts/redis_backup.sh
   #!/bin/bash

   REDIS_SOURCE=/var/lib/redis/dump.rdb
   BACKUP_DIR=/media/sas2tb/ipam/redis/backups
   BACKUP_PREFIX="redis.dump.rdb"
   DATE=`date +%Y-%m-%d`
   REDIS_DEST="$BACKUP_DIR/$BACKUP_PREFIX.$DATE"

   gzip -c $REDIS_SOURCE > $REDIS_DEST


TODOS
~~~~~

#. Backup cronjob'lari hazirlanacak.
#. Fault olan master'in manual recover edilme process'leri yazilacak. 
   - Slave olarak devam etmesi (otomatik)
   - Master'a geri dondurulmesi
#. Persistent mode <=> baslar baslamaz tamamini ram'e yazmamasi arastirilacak. 
#. Sentinel'ler icin authorization'a gerek olup olmadigi incelenecek.
#. Chef cookbook'lari hazirlanacak.
#. Yeni eklenecek slave'de yapilacaklar yazilacak (chef cookbook'u ile)
   

Testler
~~~~~~~

#. Replication calisiyor mu? redis kapatilinca, 
    - slave master'a gecti mi?
    - Yeni master'a yaziliyor mu

#. Auto failover calisiyor mu?
    - Haproxy kapatilinca VRRP ip'yi dagitiyor mu?
    - Redis'e erisim/yazma devam ediyor mu?

#. Yeni master'dan replication duzgun calisiyor mu?
    - eski master recover edildiginde:
        * Coken redis'i yeniden baslatmak yeterli, sentinel yeni master'i bulup
          eski master'i slave mode'a alip resync yapiyor.
    - Yeni eklenecek slave.
        * Yukaridakiler disinda ek bir islem yapmaya gerek yok.

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

Kaynaklar
~~~~~~~~~

#. `redhat
   <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Tuning_and_Optimizing_Red_Hat_Enterprise_Linux_for_Oracle_9i_and_10g_Databases/chap-Oracle_9i_and_10g_Tuning_Guide-Setting_Shell_Limits_for_the_Oracle_User.html>`_
#. `Highly Available Redis Cluster
   <http://www.101tech.net/2014/08/08/highly-available-redis-cluster/>`_
#. `Installing a High Availability Redis service on CentOS 6.X in Windows
   Azure:
   <https://robertianhawdon.me.uk/2014/02/11/sysops-installing-a-high-availability-redis-service-on-centos-6-x-in-windows-azure/>`
#. `Haredis: <https://github.com/falsecz/haredis>`_
