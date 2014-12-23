==================
Mysql Replication
==================

* Binary logging must be enabled on the master because the binary log is the
basis for sending data changes from the master to its slaves.
Mysql'de 3 tip replication built-in yapilabiliyor
* asynronous
* Semisynchronous
* fully synronous

Semisynchronous replication 
====================================

MySQL 5.5 supports an interface to semisynchronous replication that is
implemented by plugins a commit performed on the master side blocks before
returning to the session that performed the transaction until at least one
slave acknowledges that it has received and logged the events for the
transaction.

* 5.5'te default storage engine innodb. farkli surumlerde mysql replike
ediyorsan eski mysql sunucudaki storage engine'in innodb olup olmadigini kontrol et.

.. code:: sh

    show engines\g;

* 3'lu bir yapi dusunuyorsan, ornegin; master(1) <- Master/Slave(2) <- Slave(3)
2'den 3'e replike olabilmesi icin 2'nin my.cnf'una ekle;
5.5
log-slave-updates = ON

5.1
log-slave-updates = 1

`Kaynak: <http://michaelhallsmoore.com/blog/MySQL-Chained-Replication-Do-Not-Forget-log-slave-updates>`_

How to identify Replication Delay
MySQL replication works with two threads, IO_THREAD & SQL_THREAD. IO_THREAD
connects to a master, reads binary log events from the master as they come in
and just copies them over to a local log file called relay log. On the other
hand, SQL_THREAD reads events from a relay log stored locally on the
replication slave (the file that was written by IO thread) and then applies
them as fast as possible. Whenever replication delays, it’s important to
discover first whether it’s delaying on slave IO_THREAD or slave SQL_THREAD.

`Kaynak: <http://www.percona.com/blog/2014/05/02/how-to-identify-and-cure-mysql-replication-slave-lag>`_

 when the slave SQL_THREAD is the source of replication delays it is probably
 because of queries coming from the replication stream are taking too long to
 execute on the slave.

Mysql replikasyonunu monitör etmek için;
master'ın SHOW MASTER STATUS çıktısındakı file değeri ile (ÖR:mysql-bin.018196) 
slave'in SHOW SLAVE STATUS çıktısındakı Master_Log_File: değerinin aynı olması gerekiyor.
Bunun dışında da yakalanabilecek yerler var ama bunun kadar net değil. Örneğin
SLAVE STATUS çıktısındaki Seconds_Behind_Master değeri. Ama master'dan alınmayı
bekleyen binlog'lar varsa bu değer ne kadar geride olduğunu tam olarak
ölçemiyor.
SHOW SLAVE STATUS ciktisindaki Read_Master_Log_Pos & Exec_Master_Log_Pos
arasinda fark varsa bu SQL_THREAD'ten kaynaklanan  bir gecikme demek, sql
sorgularini slave islerken yasanan bir gecikme.


Replication Master Options and Variables 
=========================================




Replication Slave Options and Variables 
=========================================

* startup options for controlling replication slave servers.

    * --log-slave-updates: 
    Normally, a slave does not write to its own binary log any updates that are
    received from a master server. This option causes the slave to write the
    updates performed by its SQL thread to its own binary log. This is used
    when you want to chain replication servers. 

    * --master-retry-count=count
    The number of times that the slave tries to connect to the master before
    giving up. Reconnects are attempted at intervals set by the
    MASTER_CONNECT_RETRY option of the CHANGE MASTER TO statement (default 60).
    Reconnects are triggered when data reads by the slave time out according to
    the --slave-net-timeout option. The default value is 86400. A value of 0
    means “infinite”; the slave attempts to connect forever.
    This option is deprecated as of MySQL 5.6.1 and will be removed in a future
    MySQL release. Applications should be updated to use the MASTER_RETRY_COUNT
    option of the CHANGE MASTER TO statement instead.
    
    *  --read-only
    this can be useful to ensure that the slave accepts updates only from its
    master server and not from clients

    *  --replicate-do-table=db_name.tbl_name
    To specify more than one table, use this option multiple times, once for
    each table. This works for both cross-database updates and default database
    updates,

    *  --replicate-ignore-table=db_name.tbl_name
    Creates a replication filter by telling the slave SQL thread not to
    replicate any statement that updates the specified table, even if any other
    tables might be updated by the same statement.

    * relay_log_recovery
    Enables automatic relay log recovery immediately following server startup.
    The recovery process creates a new relay log file, initializes the SQL
    thread position to this new relay log, and initializes the I/O thread to
    the SQL thread position. Reading of the relay log from the master then
    continues. 

    When relay_log_recovery is enabled and the slave has stopped due to errors
    encountered while running in multi-threaded mode, you cannot execute CHANGE
    MASTER TO if there are any gaps in the log. 
      
    * sync_relay_log
    If the value of this variable is greater than 0, the MySQL server
    synchronizes its relay log to disk (using fdatasync()) after every
    sync_relay_log events are written to the relay log.
    Setting sync_relay_log to 0 causes no synchronization to be done to disk;
    in this case, the server relies on the operating system to flush the relay
    log's contents from time to time as for any other file.

    Prior to MySQL 5.6.6, 0 was the default for this variable. In MySQL 5.6. and
    later, the default is 10000.
    
    A value of 1 is the safest choice because in the event of a crash you lose at
    most one event from the relay log. However, it is also the slowest choice
    (unless the disk has a battery-backed cache, which makes synchronization very
    fast).

    *  sync_relay_log_info


* Row-based replication.  

    Tells the slave SQL thread not to update any tables in
    the database db_name. The default database has no effect.


* The slaves are configured with a series of *replicate-do-table* directives in
the my.cnf file so that only parts of the schema get replicated. The remaining
tables are modified locally, so to avoid conflicts they are not updated with
data coming from the master.
slave-net-timeout degiskeni default 1 saat, bu sure icerisinde kesinti
oldugunda slave uyanmiyor.

`Kaynak:danielschneller.com <http://www.danielschneller.com/2006/10/mysql-replication-timeout-trap.html>`_

* replication'da kullanilabilecek diger onemli parameterler;

- Master'da;
    * auto_increment_increment and auto_increment_offset 
    They are intended for use with master-to-master replication, and can be used to
    control the operation of AUTO_INCREMENT columns.

- Slave'de;
   * --slave-parallel-workers
   * --slave-pending-jobs-size-max=#
   * --slave_compressed_protocol={0|1}
   * --slave-skip-errors=[err_code1,err_code2,...|all|ddl_exist_errors]

