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

Preparing the Master for Standby Servers
----------------------------------------

#. he archive location should be accessible from the standby even when the
   master is down, i.e. it should reside on the standby server itself or
   another trusted server, not on the master server.


