Barman 2.0, backup over streaming replication is the recommended setup for
PostgreSQL 9.4 or higher.

Traditional backup via rsync /SSH is available for all versions of PostgreSQL
starting from 8.3, and it is recommended in all cases where pg_basebackup
limitations occur (for example, a very large database that can benefit from
incremental backup and deduplication).  
The reason why we recommend streaming backup is that, based on our experience,
it is easier to setup than the traditional one. 

Two typical scenarios for backups
Scenario 1: Backup via streaming protocol (streaming-only setup)
In this scenario, you will need to configure:
1. a standard connection to PostgreSQL, for management, coordination, and
   monitoring purposes
2. a streaming replication connection that will be used by both pg_basebackup
   (for base backup operations) and pg_receivexlog (for WAL streaming)

Requirements for recovery
Remote recovery is definitely the most common way to restore a PostgreSQL
server with Barman.

Pratik:


ilgili paketler:
barman-cli: Client Utilities for Barman, Backup and Recovery Manager
for PostgreSQL
barman: Backup and Recovery Manager for PostgreSQL
pgespresso92: Optional Extension for Barman

bağımlılıklar:
python-argcomplete
python-argh       
python-dateutil   
python-psycopg2   

9.2'yle baslayan (#pg_receivexlog)'a dayaniyor. 
[stream transaction logs from a PostgreSQL server]

Initialize the new PostgreSQL cluster
/etc/init.d/postgresql-9.5 initdb
/etc/init.d/postgresql-9.2 stop
cp /var/lib/pgsql/9.2/data/pg_hba.conf /var/lib/pgsql/9.5/data/pg_hba.conf 
cp /var/lib/pgsql/9.2/data/postgresql.conf /var/lib/pgsql/9.5/data/postgresql.conf

Run pg_upgrade

/usr/pgsql-9.5/bin/pg_upgrade  \
-b /usr/pgsql-9.2/bin 
-B /usr/pgsql-9.5/bin 
-d /var/lib/pgsql/9.2/data 
-D /var/lib/pgsql/9.5/data

/etc/init.d/postgresql-9.5 start

#Move new data into place:
cd /usr/local/var

   sudo -u postgres /usr/pgsql-9.2/bin/pg_upgrade
--old-datadir=/var/lib/pgsql/9.1/data --new-datadir=/var/lib/pgsql/9.2/data
--old-bindir=/usr/pgsql-9.1/bin --new-bindir=/usr/pgsql-9.2/bin

please run barman as 'barman' user
usage: barman [-h] [-v] [-c CONFIG] [-q] [-d] [-f {console}]
              
{diagnose,status,replication-status,list-server,list-files,get-wal,receive-wal,show-backup,cron,rebuild-xlogdb,switch-xlog,show-server,archive-wal,recover,backup,check,list-backup,delete}


psql -c 'SELECT version()' -U barman -h pg postgres 
PostgreSQL 9.5.5 on x86_64-pc-linux-gnu, compiled by gcc (GCC) 4.4.7 20120313
(Red Hat 4.4.7-17), 64-bit

psql -U streaming_barman -h pg -c "IDENTIFY_SYSTEM" replication=1 psql -U

streaming_barman -h pg -c ""
      systemid       | timeline |  xlogpos  | dbname 
---------------------+----------+-----------+--------
 6368754516593252106 |        1 | 0/17A1960 | 
(1 row)

echo "0" > /var/lib/barman/pg/wals/xlog.db  

barman komutlari
diagnose
status
replication-status
list-server
list-files
get-wal
receive-wal
show-backup
cron
rebuild-xlogdb
switch-xlog
show-server
archive-wal
recover
backup
check
list-backup
delete

pgespresso 
* Added support for concurrent backup of PostgreSQL 9.2 and 9.3
  servers that use the "pgespresso" extension. This feature is
  controlled by the "backup_options" configuration option (global/
  server) and activated when set to "concurrent_backup". Concurrent
  backup allows DBAs to perform full backup operations from a
  streaming replicated standby.

### Step by step guide 
#### for online;
* host tanimlari /etc/hosts'a girilir.
* kurulum
wget https://yum.postgresql.org/9.5/redhat/rhel-6-x86_64/pgdg-centos95-9.5-3.noarch.rpm
rpm -ivh pgdg-centos95-9.5-3.noarch.rpm
yum install -y barman barman-cli
yum install -y postgesql95-server rsync

* pg'de olusturulan keyler alinir 
mkdir -p ~barman/.ssh
chown barman: authorized_keys id_rsa.pub id_rsa
chown barman: ~barman/.ssh
mv authorized_keys id_rsa\* ~barman/.ssh
chmod -R go-rwx ~barman/.ssh
* yoksa burada olusturulup gonderilir.
su - barman
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod go-rwx ~/.ssh/\*
cd ~/.ssh
scp id_rsa.pub id_rsa authorized_keys postgres_sunucu:~/

* barman'a ait dosya/dizinlerin sahiplikleri duzenlenir:
chown barman: /etc/barman.\* -R
mkdir -p /data_dizini/incoming
chown -R barman: /data_dizini


* barman configuration dosyasi ornek-1
[barman]
barman_user = barman
configuration_files_directory = /etc/barman.d
barman_home = "data_dizini"
recovery_options = 
log_file = /var/log/barman/barman.log
log_level = INFO
compression = gzip
immediate_checkpoint = true
reuse_backup = link 
basebackup_retry_times =  3
basebackup_retry_sleep = 30
last_backup_maximum_age = 3 DAYS

/etc/barman.d/sunucu_hostname.conf
[sunucu_hostname]
description = "sunucu_hostname"
conninfo = host=main-db-server-ip user=postgres
ssh_command = ssh postgres@main-db-server-ip
retention_policy_mode = auto
retention_policy = RECOVERY WINDOW OF 7 days
wal_retention_policy = main
archiver = on

* postgresql server'larda yapilacaklar (restart gerektirir)
wget
https://yum.postgresql.org/9.5/redhat/rhel-6-x86_64/pgdg-centos95-9.5-3.noarch.rpm
rpm -ivh pgdg-centos95-9.5-3.noarch.rpm
yum install -y postgesql95-server rsync

wal_level = archive
archive_mode = on
archive_command = 'rsync -a %p barman@sunucu_hostname:/data_dizini/incoming/%f'
pg_hba.conf

pratikte:
1 kere hata aliyrosun, xlog.db'yi duzeltip yeniden calistirinca
calisiyor. (test edicez)

psql -Upostgres -c"select i into test from generate_series(1, 100000) i"


yapilacaklar: 
* data dizini elle olusturuldugunda, sahipligi barman'a verilecek
* centos5'le test edilecek
* sensu check'i arastirilacak

yaptigimiz tanimlar
[barman]
  * Keeping the default backup location
  * Specifying that backup space should be saved. WAL logs will be
    compressed and base backups will use incremental data copying
  * Barman will retry three times if the full backup fails halfway
    through for some reason
  * The age of the last full backup for a PostgreSQL server should not
    be older than 1 day
  * reuse the last available backup for a server and create  a hard link of the unchanged files (reduce backup time and space)
[sunucu_hostname]
   The retention_policy settings mean that Barman will overwrite older
   full backup files and WAL logs automatically, while keeping enough
   backups for a recovery window of 7 days. That means we can restore the
   entire database server to any point in time in the last seven days. For
   a production system, you should probably set this value higher so you
   have older backups on hand.

psql -Upostgres -c"select i into test from generate_series(1, 100000) i"
