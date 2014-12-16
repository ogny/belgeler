Redis Caching Duzenleme
#######################

:date: 2014-12-21
:comments: true 
:categories: redis
:tags: server





**simple write operation:**

1. The client sends a write command to the database (data is in client's memory).
2. The database receives the write (data is in server's memory).
3. The database calls the system call that writes the data on disk (data is in the kernel's buffer).
4. The operating system transfers the write buffer to the disk controller (data is in the disk cache).
5. The disk controller actually writes the data into a physical media (a magnetic disk, a Nand chip, ...).

It's time to check how Redis scores in this regard. Redis provides two
different persistence options, so we'll examine both one after the other.

1.  Snapshotting 
    The durability of snapshotting is; If the dataset is saved every 15 minutes,
    than in the event of a Redis instance crash or a more catastrophic event,
    up to 15 minutes of writes can be lost.

    if the previous snapshot was created more than 2 minutes ago and there are
    already at least 100 new writes, a new snapshot is created. Snapshots are
    produced as a single _ .rdb _ file that contains the whole dataset. The RDB
    file can not get corrupted, because it is produced by a child process in an
    append-only way, starting from the image of data in the Redis memory.

2.  on AOF (Append only file) more advanced persistence mode; is still
    advisable to also turn snapshotting on (to perform backups)
    every time a write operation that modifies the dataset in 
    memory is performed, the operation gets logged. The log is produced 
    exactly in the same format used by clients to communicate with Redis, 
    if they have some effect on actual data. 
    
    However not all the commands are logged as they are received. For instance
    blocking operations on lists are logged for their final effects as normal
    non blocking commands. Similarly INCRBYFLOAT is logged as SET, using the
    final value after the increment as payload, so that differences in the way
    floating points are handled by different architectures will not lead to
    different results after reloading an AOF file.

    Once the rewrite is terminated, the temporary file is synched on disk with
    fsync and is used to overwrite the old AOF file.

    Redis serves read operations from memory, so data on disk does not need to
    be organized for a random access pattern, but just for asequential loading
    on restart.
    




