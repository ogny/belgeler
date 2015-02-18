=================
``Postgresql HA``
=================

:date: Paz 15 Şub 2015 20:21:10 EET
:comments: true
:categories: 
:tags: 
:Author: Orkun Gunay

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
------

#. replicator kullanicisi olusturulur

#. 

`<https://gist.github.com/greinacker/4968619#file-user-sh>`_



    #. postgresql.conf buradaki gibi duzenlenir.



    #. pg_hba.conf

#. Hata log'lari;

    $HOME/pgstartup.log yaziyor sonra rotasyon basliyor 
    $HOME/9.4/data/pg_log altinda tutuluyor.




Kaynaklar:
-----------

#. `<http://www.postgresql.org/docs/9.3/>`_

#. `<http://www.rassoc.com/gregr/weblog/2013/02/16/zero-to-postgresql-streaming-replication-in-10-mins/>`_ 

#. `<http://www.postgresql.org/docs/9.1/static/ssl-tcp.html>`_
