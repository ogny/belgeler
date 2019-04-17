### Repmgr ile PG HA

* Centos 6.7'da test edilmistir.

#### Sistem yapilandirma;

```
rpm -Uvh \
http://yum.postgresql.org/9.4/redhat/rhel-6-x86_64/pgdg-centos94-9.4-1.noarch.rpm
yum install -y postgresql94-server postgresql94-contrib \
postgresql94-devel libxslt-devel libxml2-devel repmgr94 
chkconfig keepalived on
chkconfig postgresql-9.4 on
cp /etc/repmgr/9.4/repmgr.conf{,.org}
su - postgres
echo "export PATH=$PATH:/usr/pgsql-9.4/bin" >> ~/.bash_profile
source ~/.bash_profile
```

* master'de db olustur, baslat ve surumleri test et (standby'da db initialize etme)
```
initdb -D $PGDATA -A trust -U postgres
pg_ctl -D $PGDATA -l logfile -w start
psql --version
psql -U postgres -c "select version();"
repmgr --version
```

#### Repmgr yapilandirma

##### Master Server
```
vi $PGDATA/postgresql.conf
    listen_addresses='*'
    hot_standby = on
    wal_level = 'hot_standby'
    max_wal_senders = 10
#   max_replication_slots = 4
#   shared_preload_libraries = 'repmgr_funcs'
    archive_mode = on
    archive_command = 'cd .'
    wal_keep_segments = 5000
# wal_keep_segments kullanma
vi $PGDATA/pg_hba.conf
host    repmgr_db       repmgr_usr  <IP_blogu>/24         trust
host    replication     repmgr_usr  <IP_blogu>/24         trust
```

* monitoring and replication data'nin tutulmasi icin kullanici ve veri tabani
  olusmasi gerekiyor.
```
createuser -s repmgr
createdb repmgr -O repmgr
#psql -f /usr/pgsql-9.4/share/contrib/repmgr_funcs.sql repmgr
```

* repmgr.conf (master/standby) (root ile)
```
chmod 777 /var/log/repmgr
vi /etc/sysconfig/pgsql/repmgr.conf
 
cluster=test
node=1
node_name=node1
conninfo='host=<IP> repmgr user=repmgr'
pg_bindir=/usr/pgsql-9.4/bin
#use_replication_slots=1
pg_basebackup_options='--xlog-method=fetch'
logfile='/var/log/repmgr/repmgr.log'
```

* Master node register edilir.
```
repmgr  master register
```

* standby node'a pg_basebackup alinir. (rsync'le henuz basarili olamadim)
  register edilir ve baslatilir
```
repmgr --force --verbose -h <master_ip -d repmgr -U repmgr -D \
/var/lib/pgsql/9.5/data standby clone
repmgr --force --verbose -h <master_ip -d repmgr -U repmgr -D \
/var/lib/pgsql/9.5/data standby clone --rsync-only 
repmgr  standby register
pg_ctl -D $PGDATA start
```

#### Kontroller
* master'da
```
repmgr  cluster show
psql -c "SELECT pg_current_xlog_location();"
```
* slave'de
```
repmgr  cluster show
psql -c "SELECT pg_last_xlog_receive_location();"
```
* sensu-check'i aktif mi
* slave makinaya bagli eski bir data diski var mi
* disklerin son state'i nedir
* postgre db'lerde yetkili mi (degilse ver)
```
psql
\l
GRANT ALL PRIVILEGES ON DATABASE pims to postgres;


#### Geri Donus
* baglantiyi tekrar sagla
ip route del blackhole 172.27.98.90/32
* son state'e bak, 2 makina da master/master mi
crm cluster show
* etlik'i durdur
crm resource stop PGSQL
* disk bu makinada mi bir daha  kontrol et
crm_mon -1
* mevcut data diskinin adini degistir
mv /data/db/pgsql/9.5/data /data/db/pgsql/9.5/data_old
* basebackup al
repmgr --force --verbose -h 172.27.98.90 -U repmgr -d repmgr -D \
/data/db/pgsql/9.5/data standby clone
* db'yi ac
crm resource start PGSQL
* cluster'a bak
repmgr cluster show
* kontrolleri yap
  - master'da
psql -c "SELECT pg_current_xlog_location();"
  - slave'de
psql -c "SELECT pg_last_xlog_receive_location();"
* eski data disk'lerini sil
cd /data/db/pgsql/9.5/
rm -rf data_ data.old data__
* sensu check'ini geri getir













#### Diger
#. Repmgrd ile replication'u yonetme:
```  
nohup repmgrd -f /etc/repmgr/10.4/repmgr.conf --daemonize  --monitoring-history --verbose &
```  

#### hatalar 

* `pg_ctl -w stop` ile kapatmak istediginde; server does not shut down donuyorsa
```
pg_ctl  -D $PGDATA -m immediate stop
```
* Standby master'i klonladiktan sonra acilmiyor; 'FATAL:  could not open
  relation mapping file "global/pg_filenode.map": No such file or directory'
  diyor, ancak dosya orada mevcut, haklariyla ilgili bir sorun yok. Buradaki ilginclik su, aslinda postgre'nin bir calisan servisi halihazirda mevcut. oldurmek istediginde ise postmaster.pid yok diyor. Bu durumda pid'in varligini kontrol ederek postgre'yi kaldirip indirmek saglikli bir metod degil.



Not:  
---
* `wal_keep_segments` 9.4'ten itibaren mecburi degil; replication slots yerine
  kullaniliyor. Standby sunuculardan biri giderse, onun replication slot'u
  silinmeden WAL dosyalari silinmez. silmenin yolu; 
.

* Not: standby sunuculardan read-only sorgu yapabilmek icin maksimum "WAL senders"
  degeri toplam standby sunucu sayisindan fazla olmali.

repmgr 2'den yukseltme 
---

* bu yontemi izlemek icin oncelikle calisan repmgr'in duzgun olup olmadigindan
  emin olmam gerekiyor.



Not-Eski:  
---
#. pg_bindir ve logfile'i degistir
#. conn_info'nun sonuna dbname=repmgr user=repmgr koymayi unutma
#. promote_command'ta repmgr.conf path'ini belirt

#. pg_hba ve postgres.conf editlenir (autofailover'den);

#. pg_hba'daki kullanicilar olusturulur (autofailover'den)::

    psql -f /usr/pgsql-9.4/share/contrib/repmgr_funcs.sql repmgr

#. db ve kullanicisi olusturulur, funcs calistirilir.

#. master clone'lanir, (autofailover'den, verbose mode'da)
   
#. standby db'ler baslatilir:: 
   
    pg_ctl  -D $PGDATA -l logfile -w start 

#. node'lar role'lerine gore register edilir::

    repmgr -f $HOME/repmgr/repmgr.conf --verbose master register
    repmgr -f $HOME/repmgr/repmgr.conf --verbose standby register

#. node2'de::

    repmgr -d repmgr -U repmgr --verbose standby clone <master_ip>

    pg_ctl  -D $PGDATA -l logfile -w start 

#. node3'de::

    repmgr -d repmgr -U repmgr --verbose standby clone <master_ip>

    pg_ctl  -D $PGDATA -l logfile -w start 

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

#. replication'u kontrol et::

    select * from pg_stat_replication;



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

### Diger

UPDATE repmgr_fatih.repl_nodes SET type='standby' WHERE id=2;

ip route add blackhole 192.168.33.15
ip route add blackhole 192.168.33.17
export PATH=/usr/pgsql-9.5/bin:$PATH

```
psql -d pims
create table yeni_tablo(
id int PRIMARY KEY,
last_name varchar NOT NULL,
salary int
);
INSERT INTO yeni_tablo VALUES (1,'Jones',45000);
\q
```

### debian'a 9.4 kurulum icin
```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - 
```
sudo apt-get install libpq-dev=9.4* libpq5=9.4*
