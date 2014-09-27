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

