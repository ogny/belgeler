-------
Kurulum
-------

Tum Node'larda yapilacaklar
----------------------------

#. kurulum:: 

  rpm -Uvh \
  http://yum.postgresql.org/9.4/redhat/rhel-6-x86_64/pgdg-centos94-9.4-1.noarch.rpm
  yum install -y postgresql94-server postgresql94-contrib postgresql94-devel \
  screen rsync libxslt-devel libxml2-devel keepalived repmgr 
  chkconfig keepalived on
  chkconfig postgresql-9.4 on

#. bash_profile::

  su - postgres
  echo "export PATH=$PATH:/usr/pgsql-9.4/bin/" >> ~/.bash_profile
  source ~/.bash_profile

Master'de 
~~~~~~~~~

#. postgres kullanicisiyla::
  su - postgres 

==> (GERCEK MASTER'da YAPMANA GEREK YOK)::
  initdb -D /var/lib/pgsql/9.4/data -A trust -U postgres

::
  ssh-keygen -t rsa
  cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  ssh localhost
  chmod go-rwx ~/.ssh/*
  exit
  
#. root ile::

  rsync -avz ~postgres/.ssh/ 195.175.250.24:~postgres/.ssh/

------------
Yapilandirma
------------

Master'de 
~~~~~~~~~
::

  su - postgres 
  mkdir $HOME/repmgr
  vi $HOME/repmgr/repmgr.conf

    cluster=ulus
    node=1 
    node_name=node1
    conninfo='host=195.175.250.23 user=postgres dbname=ttipam'
    pg_bindir=/usr/pgsql-9.4/bin/
    rsync_options=--archive --checksum --compress --progress --rsh="ssh -o \"StrictHostKeyChecking no\"
    ssh_options=-o "StrictHostKeyChecking no"
    failover=manual
    logfile='/var/lib/pgsql/9.4/data/pg_log/repmgr.log'
    loglevel=DEBUG
    logfacility=STDERR

  ==> (GERCEK MASTER'da YAPMANA GEREK YOK)
  vi $PGDATA/pg_hba.conf 

    local   all             postgres                                ident
    local   all             all                                     ident
    host    all             all             127.0.0.1/32            md5
    host    all             all             ::1/128                 md5
    host    all             all             127.0.0.1/32            trust
    host    all             all             ::1/128                 trust
    host    all             all             195.175.249.64/26       trust
    host    all             all             195.175.250.0/27        trust
    host    all             all             195.175.250.32/27       trust
    host    all             all             195.175.250.64/27       trust
    host    all             all             195.175.251.1/27        trust
    host    all             all             85.105.152.113/32       trust
    host    all             all             85.105.152.114/32       trust

  cp $PGDATA/postgresql.conf{,.org}
  vi $PGDATA/postgresql.conf

    listen_addresses='*'
    wal_level = 'hot_standby'
    archive_mode = on
    archive_command = 'cd .'   # we can also use exit 0, anything that
                               # just does nothing
    max_wal_senders = 10
    wal_keep_segments = 5000   # 80 GB required on pg_xlog
    hot_standby = on
    shared_preload_libraries = 'repmgr_funcs'

  ==> (GERCEK MASTER'da YAPMANA GEREK YOK)
  createdb -O posgtres ttipam

  pg_ctl start -m fast
  psql -f /usr/pgsql-9.4/share/contrib/repmgr_funcs.sql ttipam

#. node'lar role'lerine gore register edilir::

    repmgr -f $HOME/repmgr/repmgr.conf --verbose master register
    repmgr -f $HOME/repmgr/repmgr.conf --verbose standby register

#. Cluster'in durumunu takip etme::

    repmgr cluster show -f repmgr/repmgr.conf
    SELECT * from repmgr_ulus.repl_nodes

Slave'de 
~~~~~~~~~

::
  mkdir $HOME/repmgr
  vi $HOME/repmgr/repmgr.conf

    cluster=ipamulus
    node=2
    node_name=node2
    conninfo='host=195.175.250.24 user=postgres dbname=ttipam'
    pg_bindir=/usr/pgsql-9.4/bin/
    rsync_options=--archive --checksum --compress --progress --rsh="ssh -o \"StrictHostKeyChecking no\"
    ssh_options=-o "StrictHostKeyChecking no"
    failover=manual
    logfile='/var/lib/pgsql/9.4/data/pg_log/repmgr.log'

#. repmgr ile master clone'lanir, ve standby db baslatilir::
    
    repmgr -d ttipam -U postgres --verbose standby clone 195.175.250.23
    pg_ctl start -m fast


----------
Keepalived
----------

Tum Node'larda
~~~~~~~~~~~~~~

PG Kontrol Betigi::

    su - postgres
    mkdir $HOME/scripts
    vi $HOME/scripts/check_postgres.sh

        #!/bin/bash
        
        if [ -f /var/lib/pgsql/9.4/data/postmaster.pid ] ; then
                echo "Postgresql is accessible"
                exit 0
        else
                echo "Postgresql is not accessible"
                exit 1
        fi

    chmod +x $HOME/scripts/check_postgres.sh

Yapilandirma
------------

::
    mv /etc/keepalived/keepalived.conf{,.org}
    vi /etc/keepalived/keepalived.conf

    ! Configuration File for keepalived

    global_defs {
        notification_email {
        devops@egemsoft.net
        }
        notification_email_from root@ipam-ulus-db-1
        smtp_server localhost
        smtp_connect_timeout 30
    }
    vrrp_script chk_postgres {
    script "/var/lib/pgsql/scripts/check_postgres.sh"
        interval 1
        fall 10
    }
    vrrp_instance VI_0 {
        state BACKUP # Standby'da BACKUP
        interface eth0
        virtual_router_id 40
        priority 101 # MASTER'daki deger BACKUP'lardan buyuk olacak
        advert_int 1
        nopreempt # Sadece MASTER'da
        virtual_ipaddress {
        195.175.250.15
        }
        track_script {
            chk_postgres
        }
        notify_fault "/var/lib/pgsql/scripts/promote.sh"
    }

    /etc/init.d/keepalived start

Master'da
~~~~~~~~~

::
    vi $HOME/scripts/promote.sh

        #!/bin/bash

        runuser -l postgres -c "ssh -o StrictHostKeyChecking=no -f
        195.175.250.24
        /usr/pgsql-9.4/bin/repmgr standby promote -f
        /var/lib/pgsql/repmgr/repmgr.conf"

    chmod +x $HOME/scripts/promote.sh

-----------
Geri Donus;
-----------

#. Master standby'a cekilir:: 
 
    repmgr -d ttipam -U postgres --verbose --force standby clone 195.175.250.24

#. node1'i ac, node2'yi durdur, node1'i promote et

node1::
   pg_ctl start -m fast

node2::
   pg_ctl stop -m fast

node1::
   repmgr standby promote -f $HOME/repmgr/repmgr.conf

node2::
    repmgr -d ttipam -U postgres --verbose --force standby clone 195.175.250.23
    pg_ctl start -m fast

#. Her iki node'da cluster'in durumu kontrol edilir::

   repmgr -f $HOME/repmgr/repmgr.conf cluster show

#. Elle yapilacaklar;
   
ulus'u cluster'i fatih'e atama::

   repmgr -f $HOME/repmgr/repmgr.conf standby promote

master'i takip ettirme::

   repmgr -f $HOME/repmgr/repmgr.conf standby follow

--------    
Hatalar;
--------    

#. cluster'in yapisi bozuldugunda, master'da cluster'i sil, master node'u yeniden
   kaydet master'i standby'a clone'la, slave node'u yeniden kaydet ::

    psql -d ttipam
    DROP SCHEMA IF EXISTS repmgr_fatih CASCADE;
    \q

    SELECT * from repmgr_fatih.repl_nodes;

#. repmgr3'te clone pg_basebackup ile aliniyor, onun erisebilmesi icin pg_hba.conf'ta su satir olmali::
   host    replication     all             <slave_ip>   trust


