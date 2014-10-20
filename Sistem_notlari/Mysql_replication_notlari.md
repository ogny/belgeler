* Binary logging must be enabled on the master because the binary log is the
basis for sending data changes from the master to its slaves.
Mysql'de 3 tip replication built-in yapilabiliyor
* asynronous
* Semisynchronous
* fully synronous
### Semisynchronous replication 
MySQL 5.5 supports an interface to semisynchronous replication that is
implemented by plugins a commit performed on the master side blocks before
returning to the session that performed the transaction until at least one
slave acknowledges that it has received and logged the events for the
transaction.

* 5.5'te default storage engine innodb. farkli surumlerde mysql replike
ediyorsan eski mysql sunucudaki bstorage engine'in innodb olup olmadigini kontrol et.
```
engines\g
```

* 3'lu bir yapi dusunuyorsan, ornegin; aster(1) <- Master/Slave(2) <- Slave(3)
2'den 3'e replike olabilmesi icin 2'nin my.cnf'una ekle;
5.5
log-slave-updates = ON

5.1
log-slave-updates = 1
[kaynak:](http://michaelhallsmoore.com/blog/MySQL-Chained-Replication-Do-Not-Forget-log-slave-updates)

How to identify Replication Delay
MySQL replication works with two threads, IO_THREAD & SQL_THREAD. IO_THREAD
connects to a master, reads binary log events from the master as they come in
and just copies them over to a local log file called relay log. On the other
hand, SQL_THREAD reads events from a relay log stored locally on the
replication slave (the file that was written by IO thread) and then applies
them as fast as possible. Whenever replication delays, it’s important to
discover first whether it’s delaying on slave IO_THREAD or slave SQL_THREAD.
[kaynak:](http://www.percona.com/blog/2014/05/02/how-to-identify-and-cure-mysql-replication-slave-lag)

 when the slave SQL_THREAD is the source of replication delays it is probably
 because of queries coming from the replication stream are taking too long to
 execute on the slave.

Mysql replikasyonunu monitör etmek için;
master'ın SHOW MASTER STATUS çıktısındakı file değeri ile (ÖR:mysql-bin.018196) 
slave'in SHOW SLAVE STATUS çıktısındakı Master_Log_File: değerinin aynı olması gerekiyor.
Bunun dışında da yakalanabilecek yerler var ama bunun kadar net değil. Örneğin SLAVE STATUS çıktısındaki Seconds_Behind_Master değeri. Ama master'dan alınmayı bekleyen binlog'lar varsa bu değer ne kadar geride olduğunu tam olarak ölçemiyor.
SHOW SLAVE STATUS ciktisindaki Read_Master_Log_Pos & Exec_Master_Log_Pos
arasinda fark varsa bu SQL_THREAD'ten kaynaklanan  bir gecikme demek, sql
sorgularini slave islerken yasanan bir gecikme.

* The number of times that the slave tries to connect to the master before giving
up. Reconnects are attempted at intervals set by the MASTER_CONNECT_RETRY
option of the CHANGE MASTER TO statement (default 60). Reconnects are triggered
when data reads by the slave time out according to the --slave-net-timeout
option. The default value is 86400. A value of 0 means “infinite”; the slave
attempts to connect forever.

* Onemli:
slave-net-timeout degiskeni default 1 saat, bu sure icerisinde kesinti
oldugunda slave uyanmiyor.
[Kaynak:](http://www.danielschneller.com/2006/10/mysql-replication-timeout-trap.html]



