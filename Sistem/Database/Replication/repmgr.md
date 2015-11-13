### Repmgr ile PG HA

#### Sistem yapilandirma;

```
rpm -Uvh \
http://yum.postgresql.org/9.4/redhat/rhel-6-x86_64/pgdg-centos94-9.4-1.noarch.rpm
yum install -y postgresql94-server postgresql94-contrib \
postgresql94-devel libxslt-devel libxml2-devel repmgr94 
chkconfig keepalived on
chkconfig postgresql-9.4 on
su - postgres
echo "export PATH=$PATH:/usr/pgsql-9.4/bin" >> ~/.bash_profile
source ~/.bash_profile
```

* master'de db olustur, baslat ve surumleri test et

```
initdb -D $PGDATA -A trust -U postgres
pg_ctl -D $PGDATA -l logfile start
psql --version
psql -U postgres -c "select version();"
repmgr --version
repmgrd --version
```

#### Repmgr yapilandirma

##### Master Server

```
cp /etc/repmgr/9.4/repmgr.conf /etc/repmgr/repmgr.conf
vi $PGDATA/postgresql.conf
    listen_addresses='*'
    hot_standby = on
    wal_level = 'hot_standby'
    max_wal_senders = 10
# repmgrd kullanilacaksa
    shared_preload_libraries = 'repmgr_funcs'
# zorunlu degil
    archive_mode = on
    archive_command = 'cd .'
# durum degisiyor, kapatilabilir.
wal_keep_segments = 500
```

*  monitoring and replication data'nin tutulmasi icin kullanici ve veri tabani
   olusmasi gerekiyor.
```
psql -U postgres -c "createuser -s repmgr"
psql -U postgres -c "createdb repmgr -O repmgr"
```

* repmgr.conf (master/standby)

```
cluster=test
node=1
node_name=node1
conninfo='host=<IP> user=repmgr dbname=repmgr'
pg_bindir=/usr/pgsql-9.4/bin
use_replication_slots=1
pg_basebackup_options='--xlog-method=s'
```

* Node'lari master/standby olarak kaydet ve kontrol et

```
repmgr master register
repmgr standby register
repmgr cluster show
```




Not:  
---
* `wal_keep_segments` 9.4'ten itibaren mecburi degil; replication slots can be
  used instead (see below).

* Not: standby sunuculardan read-only sorgu yapabilmek icin maksimum "WAL senders"
  degeri toplam standby sunucu sayisindan fazla olmali.

#. pg_bindir ve logfile'i degistir
#. conn_info'nun sonuna dbname=repmgr user=repmgr koymayi unutma
#. promote_command'ta repmgr.conf path'ini belirt

#. pg_hba ve postgres.conf editlenir (autofailover'den);

#. pg_hba'daki kullanicilar olusturulur (autofailover'den)::

    psql -f /usr/pgsql-9.4/share/contrib/repmgr_funcs.sql repmgr

#. db ve kullanicisi olusturulur, funcs calistirilir.

#. master clone'lanir, (autofailover'den, verbose mode'da)
   
#. standby db'ler baslatilir:: 
   
    pg_ctl start -m fast

#. node'lar role'lerine gore register edilir::

    repmgr -f $HOME/repmgr/repmgr.conf --verbose master register
    repmgr -f $HOME/repmgr/repmgr.conf --verbose standby register

#. node2'de::

    repmgr -d repmgr -U repmgr --verbose standby clone <master_ip>

    pg_ctl start -m fast

#. node3'de::

    repmgr -d repmgr -U repmgr --verbose standby clone <master_ip>

    pg_ctl start -m fast

#. Cluster'in durumunu takip etme::

    repmgr cluster show -f repmgr/repmgr.conf


Geri donus
==========

node2'yi master'dan slave'e cekmek;
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. node1 standby'a cekilir::

    repmgr -d repmgr  -U repmgr --verbose --force standby clone <node2_ip>

#. node1'i ac, node2'yi durdur, node1'i promote et::

    repmgr standby promote -f $HOME/repmgr/repmgr.conf

#. node3'u node1'e follow ettir::

    repmgr -f $HOME/repmgr/repmgr.conf standby follow

#. node2'den node1'i klonla, node2'yi baslat::

    repmgr -d repmgr  -U repmgr --verbose --force standby clone <node1_ip>

#. replication'u kontrol et

#. Repmgrd calistirma::

    repmgrd -f repmgr/repmgr.conf --daemonize -> $PGDATA/pg_log/repmgr.log 2>&1


uzerinde calisilacaklar
=======================

repl_status monitoring'i kullanmadim, ayrica log'da bazi hatalar da gordum,
vakit oldugunda ona donmek istiyorum.

Ekler;
======

#. test verisi olusturma::

    create table <tablo_adi> as select s, md5(random()::text) from generate_Series(1,5) s;
    
#. test verisini silme::

    drop schema public cascade;

    create schema public;

#. olusan hatalar
=================

1) node2 acilmiyor, hata;
   postgresql invalid resource manager ID in primary checkpoint record could
   not locate a valid checkpoint record
 
tekrar node1'den datayi cektigimde calisti

2) node1 clonle'lamada hata olduysa;
Can't start backup: ERROR:  a backup is already in progress
HINT:  Run pg_stop_backup() and try again.

node2'deki backup process'ini durdurup node1'de clone'lamayi yeniden baslat;
psql -x -d test -c "select pg_stop_backup()";'
repmgr -d repmgr  -U repmgr --verbose --force standby clone <node2_ip>

3) reverse'te bazen node1'i master'a donustururken promote calismiyor, node2'yi
tekrar acip kapatip promote et.

4) slave WAL'i master'dan daha ilerideyse 
could not receive data from WAL stream: ERROR:  requested starting point
0/6E000000 is ahead of the WAL flush position of this server 0/5A018508

son master olan makinadan clone alinip yeniden baslatilir

5) slave yine ileride, yeni master'dan klon'ladim

invalid xl_info in checkpoint record
could not locate required checkpoint record
If you are not restoring from a backup, try removing the file "/var/lib/pgsql/9.4/data/backup_label".
startup process (PID 12749) exited with exit code 1
aborting startup due to startup process failure

#. Klonladigimdaki log;
started streaming WAL from primary at 0/84000000 on timeline 1
redo starts at 0/84000028
consistent recovery state reached at 0/840000F0

master'i follow ettigimdeki log;

redo starts at 0/84000028
consistent recovery state reached at 0/85000060
database system is ready to accept read only connections
record with zero length at 0/85000060
started streaming WAL from primary at 0/85000000 on timeline 1

6)  repmgr needs parameter 'wal_keep_segments' to be set to 5000 or greater
(see the '-w' option or edit the postgresql.conf of the PostgreSQL master.)


7) herhangi bir makinayi unregister etmek istediginde; 
master'da replikasyonun tutuldugu db'ye gecip
DELETE FROM repmgr_<cluster_name>.repl_nodes WHERE name = '<node_name>';

8) uzak makinayi promote ederken asagidaki hatayi veriyor::

    [WARNING] reconnect_attemp s/1: Unknown name/value pair!


promote script'inde follow'lar
sleep 5

runuser -l postgres -c "ssh -o StrictHostKeyChecking=no -f 195.175.249.108
'/usr/pgsql-9.4/bin/repmgr -f /var/lib/pgsql/repmgr/repmgr.conf standby
follow'"
runuser -l postgres -c "ssh -o StrictHostKeyChecking=no -f 195.175.250.47
'/usr/pgsql-9.4/bin/repmgr -f /var/lib/pgsql/repmgr/repmgr.conf standby
follow'"
runuser -l postgres -c "ssh -o StrictHostKeyChecking=no -f 195.175.250.49
'/usr/pgsql-9.4/bin/repmgr -f /var/lib/pgsql/repmgr/repmgr.conf standby follow'

        
