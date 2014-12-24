==================
Mysql Replication
==================

* By default, MySQL binary logging and replication is statement-based: when the
  master server commits a change, it writes the SQL statement into its binary
  log, and any slaves that replicate it execute the same SQL statement into
  their own database.

* In Mixed Mode replication, most queries are replicated by statement. But
  transactions MySQL knows are non-deterministic are replicated by row.



`Kaynak:wingtiplabs <http://mysql.wingtiplabs.com/documentation/row639ae/configure-row-based-or-mixed-mode-replication>`_

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

* Row-based replication.  

    Tells the slave SQL thread not to update any tables in
    the database db_name. The default database has no effect.


* The slaves are configured with a series of *replicate-do-table* directives in
the my.cnf file so that only parts of the schema get replicated. The remaining
tables are modified locally, so to avoid conflicts they are not updated with
data coming from the master.
slave-net-timeout degiskeni default 1 saat, bu sure icerisinde kesinti
oldugunda slave uyanmiyor.

`Kaynak: <http://www.danielschneller.com/2006/10/mysql-replication-timeout-trap.html>`_


Advantages and Disadvantages of Statement-Based and Row-Based Replication
==========================================================================

For most users, the mixed replication format should provide the best combination of data integrity and performance.

Disadvantages of statement-based replication
------------------------------------------------

Any nondeterministic behavior is difficult to replicate Examples of such DML (Data Modification Language) statements.

* DELETE and UPDATE statements that use a LIMIT clause without an ORDER BY are nondeterministic.

* INSERT ... SELECT requires a greater number of row-level locks than with
row-based replication.

* UPDATE statements that require a table scan (because no index is used in the
WHERE clause) must lock a greater number of rows than with row-based
replication.

* For InnoDB: An INSERT statement that uses AUTO_INCREMENT blocks other
nonconflicting INSERT statements.

* For complex statements, the statement must be evaluated and executed on the
slave before the rows are updated or inserted. With row-based replication, the
slave only has to modify the affected rows, not execute the full statement.

* Table definitions must be (nearly) identical on master and slave.

Advantages of Row-Based Replication
------------------------------------------------

* All changes can be replicated. This is the safest form of replication.

* In MySQL 5.1.14 and later, the mysql database is not replicated. The mysql
  database is instead seen as a node-specific database. Row-based replication
  is not supported on tables in this database. Instead, statements that would
  normally update this information—such as GRANT, REVOKE and the manipulation
  of triggers, stored routines (including stored procedures), and views—are all
  replicated to slaves using statement-based replication.

( Buradan row-statement beraber replice etmenin neden uygun cozum oldugu
anlasiliyor.)

Disadvantages of Row-Based Replication
------------------------------------------------

* You cannot examine the logs to see what statements were executed, nor can you
  see on the slave what statements were received from the master and executed.

However, beginning with MySQL 5.1.29, you can see what data was changed using
mysql binlog with the options --base64-output=DECODE-ROWS and --verbose.





