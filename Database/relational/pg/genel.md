---
title: postgre notlari
---




* Guncel surum:
```bash
yum install \
https://yum.postgresql.org/9.6/redhat/rhel-6-x86_64/pgdg-centos96-9.6-3.noarch.rpm
```
* WAL backup'lama point-in-time recovery yapma imkani da sagliyor
* pg_basebackup: binary dosya, tum db'leri birarada alabiliyorsun
* pg_ctl'i kullanabilmek icin .bash_profile'a eklenecek path:
`/usr/pgsql-9.4/bin/`
* pg_config'i bulamiyorsa::
`ln -s /usr/pgsql-9.4/bin/pg_config /usr/local/sbin/pg_config`
* veri tabani kullanicisiyla baglanmadiginda aldigin hata
pdns=> DROP DATABASE pdns;
ERROR:  must be owner of database pdns

### DB olusturma baslatma ve durdurma

* servis olarak calistiracaksan (acilista baslatma gibi avantajlari var)
  scrtipti ilk olusturuken root oldugundan sahibi root oluyor. dizin olustuktan
  sonra sahipligini guncelle

```bash
chown postgres: /var/lib/pgsql -R
```

* centos
```bash
initdb -D $PGDATA -A trust -U postgres
pg_ctl -D $PGDATA -l logfile -w start
pg_ctl -D $PGDATA -m immediate stop
```

* Debian
```bash
binary dizini: /usr/lib/postgresql/9.5/bin/
pg_createcluster 9.5 main --start
```


### Db silme
```bash
dropdb <db_adi>
psql -U postgres -c "drop database <db_adi>"
```
* error: database is being accessed by other users
```bash
select pg_terminate_backend(pid)
from pg_stat_activity
where datname='<db_adi>';
DROP DATABASE "<db_adi>";
```

* yeni veri tabani - kullanici ekleme
```bash
CREATE USER kullanici_adi WITH PASSWORD '<parola>';
CREATE DATABASE a
    ENCODING = 'UTF8'
    TABLESPACE = pg_default
    LC_COLLATE = 'tr_TR.UTF-8'
    LC_CTYPE = 'tr_TR.UTF-8'
    CONNECTION LIMIT = -1
    TEMPLATE = template0;
GRANT ALL PRIVILEGES ON DATABASE <db_adi> to <kullanici_adi>;
```
* auto-increment'i arastir.


#. Drop all tables in postgresql?

```
drop schema public cascade;
create schema public;
```
[kaynak](http://stackoverflow.com/questions/3327312/drop-all-tables-in-postgresql)

`DROP TABLE` : remove a table
#. Dump and restore:
```
pg_dump "veri_tabani" > "dosya.sql"
psql "veri_tabani" < "dosya.sql"
```
#. Restore:
```
PGUSER=postgres pg_dumpall -h <uzak_sunucu> > db.out
psql -U <veri_tabani_sahibi> <veri_tabani_adi> -f dosya.sql
psql dbname < infile
pg_restore -U <veri_tabani_sahibi> -d db.out -v
```

#. kullanicinin parolasini guncelleme ve superuser yetkisi verme::
```
ALTER USER <kullanici_adi> WITH PASSWORD '<newpassword>';
ALTER USER <kullanici_adi> WITH SUPERUSER;
```

#. Tablo olusturma calismasi::
```
create table employees(
id int PRIMARY KEY,
last_name varchar NOT NULL,
salary int
);
INSERT INTO employees VALUES (1,'Jones',45000);
INSERT INTO employees VALUES (2,'Adams',50000);
INSERT INTO employees VALUES (3,'Williams',37000);
```

#. farkli bir port / disk'ten baslatma::

pg_ctl -D <dizin> -o "-F -p <port>" -w start

#. Debian'da::

pg_ctl -D <dizin>  -o '--config-file=/etc/postgresql/9.3/main/postgresql.conf' -w start

Kaynak
======

`Views <http://postgresguide.com/SQL/views.html>_`
`CREATE FUNCTION: return types <http://www.postgresqlforbeginners.com/2010/11/create-table-and-constraints.html>_`


#### Kullanici haklari

Tanimlar
========

#. Users are the same thing as roles. A user is a role with the LOGIN
   attribute... roles with the NOLOGIN attribute correspond to what we used to
   call groups.

#. kullanicinin spesifik db'ye erisimi::

    CREATE USER <kullanici> WITH PASSWORD '';

#. db'lere erisim kapatilip spesifik user icin acilir::

   REVOKE connect ON DATABASE <veri_tabani> FROM PUBLIC;
   GRANT connect ON DATABASE <veri_tabani> to <kullanici>;

#. kullaniciyla ilgili login'de su hatayi alirsan::

    FATAL:  password authentication failed for user "<Kullanici>"
    DETAIL:  User "<Kullanici>" has an expired password.
    ALTER ROLE <Kullanici> VALID UNTIL 'infinity';

WAL - Continous Archiving
========



`Kaynak: depesz <http://www.depesz.com/2011/07/14/write-ahead-log-understanding-postgresql-conf-checkpoint_segments-checkpoint_timeout-checkpoint_warning/>`_

#. network'ten gz dump'i import etme::

    ssh <kullanici>@<sunucu_ip> "gunzip -c dosya.sql.gz" | psql <import_edilecek_veri_tabani>

#. Read-only user'a permission verme

  "select 'grant select on ' || tablename || ' to jasperuser;' from pg_tables where schemaname = 'public'"

* Centos binary'lerin oldugu path `/usr/pgsql-9.4/bin/`

* read-only kullanici olusturma

``` sql
CREATE USER <kullanici> ENCRYPTED PASSWORD '<parola>';
GRANT CONNECT ON DATABASE <veri_tabani> TO <kullanici>;
GRANT USAGE ON SCHEMA public TO <kullanici>;
\c <veri_tabani>
GRANT SELECT ON ALL TABLES IN SCHEMA public TO <kullanici>;
GRANT SELECT ON ALL SEQUENCES IN SCHEMA public TO <kullanici>;
GRANT EXECUTE ON ALL FUNCTIONS IN SCHEMA public TO <kullanici>;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO <kullanici>;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON SEQUENCES TO <kullanici>;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT EXECUTE ON FUNCTIONS TO <kullanici>;
```

#### HATALAR

* role "" cannot be dropped because some objects depend on it
Hangi db'ye ait objelere depend ediyorsa, o db'ye olan erisim geri alinir.
```
REVOKE CONNECT ON DATABASE <db> FROM <user>;
drop owned by <user>;
drop user <user>;
```
Halen silinemiyorsa, db'yi silersek kullanici siliniyor.
db silinmeden nasil cozulur, henuz bilmiyorum.


* psql'de çıktıları dikey olarak görüntülemek için
```
\x on
```

* tarihi ogrenme:

`SELECT EXTRACT(TIMEZONE FROM now())/3600.0;`

SELECT pg_terminate_backend(pid)
    FROM pg_stat_activity
    WHERE datname = 'ii'
    AND pid <> pg_backend_pid()
    AND application_name !~ '(?:psql)|(?:pgAdmin.+)'
    AND state in ('idle', 'idle in transaction', 'idle in transaction (aborted)', 'disabled')
    AND state_change < current_timestamp - INTERVAL '5' MINUTE;

* md5 ile alma
```
U=u0; P=foobar; echo -n md5; echo -n $P$U | md5sum | cut -d' ' -f1 -
```

* postgresql error PANIC: could not locate a valid checkpoint record
  invalid primary secondary checkpoint record
```
/usr/pgsql-9.5/bin/pg_resetxlog -f $PGDATA
```
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

barman
barman-cli
python-argcomplete
python-argh



