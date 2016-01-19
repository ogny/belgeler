=================
``Postgresql HA``
=================

:date: Paz 15 Şub 2015 20:21:10 EET
:comments: true
:categories: 
:tags: 
:Author: Orkun Gunay

#. Calismaya baslamadan once iptables kurallarinin silindigig ve selinux'un
   kapatildigi kontrol edilmeli. (kalici olarak /etc/sysconfig altindan
   duzenlenmeli.)

.. code-block:: sh

    iptables -L

    setenforce 0


REPMGR ile
==========

#. pgdg repo eklenir (Kaynaklardaki unixmen linkinden)

Gerekli paketlerin kurulur

.. code-block:: sh

    yum install -y repmgr postgresql94-server postgresql94-contrib screen rsync

#. standby node'lar icin repmgr.conf'da belirlenenen isimler standby node'larin
   hosts dosyasina yazilir.

#. repo'dan kurulan paketin binary dosya yolu pgsql bash_profile'ina eklenir ve
   dosya source edilir.;

.. code-block:: sh

   export PATH=$PATH:/usr/pgsql-9.4/bin/

   source ~/.bash_profile

#. repmgr.conf dosyasi repmgr diziniyle birlikte olusturulur. 
   
.. code-block:: sh

   su - postgres -s /bin/bash --command='mkdir -p /var/lib/pgsql/repmgr/ \
   ; vi /var/lib/pgsql/repmgr/repmgr.conf'


#. repmgr_funcs'i calistirma (master'da);

.. code-block:: sh

    psql -f /usr/pgsql-9.4/share/contrib/repmgr_funcs.sql repmgr

#. witness initialize

.. code-block:: sh

   repmgr -f repmgr/repmgr.conf -D $PGDATA -d repmgr -U repmgr -h \
   <master_ip> witness create

#. repmgrd calistirma (standby ve witness'ta)

.. code-block:: sh

   repmgrd -f $HOME/repmgr/repmgr.conf --daemonize -> \ 
   $PGDATA/pg_log/repmgr.log 2>&1

#. Servisi postgres kullanicisiyla yeniden baslatma;

.. code-block:: sh

   pg_ctl -D $PGDATA restart

#. Log'larin izlenmesi;

.. code-block:: sh

   su - postgres -s /bin/bash --command='tailf $HOME/repmgr/repmgrd.log'

Uzerinde calisilanlar;
----------------------

#. repmgr'in autofailover_quick_setup dokumaninda postgresql.conf'a asagidaki
   parametrenin girilmesi oneriliyor.

.. code-block:: sh

    shared_preload_libraries = 'repmgr_funcs'  
   
Failover
---------

#. Standby'lardan bir tanesini yeni master yaptigimizda diger standby'lar onu
   otomatik olarak bulup izliyor. (2'den 3'e test edildi.)

#. Dokumanda diger standby sunuculara yeni master'i takip etmeleri icin
   asagidaki komutun girilmesi gerektigi yaziyor. Ancak testlerde buna gerek
   olmadigini gordum. node3 yeni master node2'yi otomatik takip etmeye
   basladi. 

.. code-block:: sh

    repmgr -f  $HOME/repmgr/repmgr.conf --verbose standby follow


Manuel yoldan cluster
=====================

Reverse Gecisler
-----------------

Old master ayaga kaldirilinca new master'dan slave'e gecis;
-----------------------------------------------------------

#. trigger dosyasi silinir.

#. Servis kapatilir, kapanmiyorsa postgres kulllanicisinin pid'i oldurulur.

.. code-block:: sh

   service postgresql-9.4 stop

   killall -u postgres

#. recovery.done recovery.conf ismiyle yedeklenir.

.. code-block:: sh

   mv /var/lib/pgsql/9.4/data/recovery.done ~/recovery.conf

#. data dizinini silinir.

.. code-block:: sh

   rm -rf /var/lib/pgsql/9.4/data

#. Master'dan backup alinir;

.. code-block:: sh

   su - postgres -s /bin/bash --command='/usr/bin/pg_basebackup -h \
   <Master IP> -D /var/lib/pgsql/9.4/data -P -U replicator \
   --xlog-method=stream'

#. posgresql.conf'ta hot_standby acilir

#. recovery.conf data dizinine geri tasinir.

#. servis yeniden baslatilir

.. code-block:: sh

   service postgresql-9.4 start

#. Replication'un baslayip baslamadigi kontrol edilir;

.. code-block:: sh

    ps -ef |grep receiver

#. servis yeniden baslatilir

.. code-block:: sh

   service postgresql-9.4 restart


Teori
=====

Log-Shipping Standby Servers
----------------------------

#. 

Streaming Replication
---------------------

#. Avantaji; primary'deki WAL dosyasinin dolmasini beklemeden transfer eder.

#. With streaming replication, archive_timeout is not required to reduce the
   data loss window.

#. If you use streaming replication without file-based continuous archiving,
   you have to set wal_keep_segments in the master to a value high enough to
   ensure that old WAL segments are not recycled too early, while the standby
   might still need them to catch up.

#. When the standby is started and primary_conninfo is set correctly, the
   standby will connect to the primary after replaying all WAL files available
   in the archive. If the connection is established successfully, you will see
   a walreceiver process in the standby, and a corresponding walsender process
   in the primary.

 
Preparing the Master for Standby Servers
----------------------------------------

#. he archive location should be accessible from the standby even when the
   master is down, i.e. it should reside on the standby server itself or
   another trusted server, not on the master server.

#. create a role and provide a suitable entry or entries in pg_hba.conf with
   the database field set to replication.

#. ensure max_wal_senders is set to a sufficiently large value in the
   configuration file of the primary server

Setting Up a Standby Server
----------------------------

#.  Create a recovery command file recovery.conf in the standby's cluster data
    directory, and turn on standby_mode. 
    
#.  Set restore_command to a simple command to copy files from the WAL archive. 

#. If you plan to have multiple standby servers for high availability purposes,
   set recovery_target_timeline to latest, to make the standby server follow
   the timeline change that occurs at failover to another standby.

#. If you want to use streaming replication, fill in primary_conninfo with a
   libpq connection string, including the host name (or IP address) and any
   additional details needed to connect to the primary server.

#. If you're setting up the standby server for high availability purposes, set
   up WAL archiving, connections and authentication like the primary server,
   because the standby server will work as a primary server after failover.

Failover
--------

#. If the primary server fails and the standby server becomes the new primary,
   and then the old primary restarts, you must have a mechanism for informing
   the old primary that it is no longer the primary. This is sometimes known as
   STONITH (Shoot The Other Node In The Head

Uygulama
-----------

#. TCP connection'und SSL tanimlanmasi

    #. postgresql.conf'ta SSL  ayarlardi duzenlenir

#. /var/lib/pgsql/9.4/data/ altinda duzenlenecekler;

Master (islerin tamami icin link mevcut)
----------------------------------------

#. replicator kullanicisi olusturulur

#.  `<https://gist.github.com/greinacker/4968619#file-user-sh>`_

    #. postgresql.conf buradaki gibi duzenlenir.

    #. pg_hba.conf

#. Hata log'lari;

    $HOME/pgstartup.log yaziyor sonra rotasyon basliyor 
    $HOME/9.4/data/pg_log altinda tutuluyor.


CHEF Provisioning
-----------------

Kaynaklar:
-----------

#. `Paketlerin kurulumu icin; <http://www.unixmen.com/postgresql-9-4-released-install-centos-7/>`_

#. `Repmgr Autofailover: <https://raw.githubusercontent.com/2ndQuadrant/repmgr/master/autofailover_quick_setup.rst>`_

#. `Repmgr Quickstart: <https://raw.githubusercontent.com/2ndQuadrant/repmgr/master/QUICKSTART.md>`_

#. `manuel streaming raplication: <http://www.rassoc.com/gregr/weblog/2013/02/16/zero-to-postgresql-streaming-replication-in-10-mins/>`_ 

#. `ssl ile replication: <http://www.postgresql.org/docs/9.1/static/ssl-tcp.html>`_

#. `Chef postgresql cookbook'u: <https://github.com/hw-cookbooks/postgresql>`_

