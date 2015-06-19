#### Kurulu Versiyonlar;

mongodb-org-server-2.6.3-1.x86_64
mongodb-org-tools-2.6.3-1.x86_64
mongodb-org-2.6.3-1.x86_64
mongodb-org-mongos-2.6.3-1.x86_64
mongodb-org-shell-2.6.3-1.x86_64

#### Yedekleme

* Mongodump ile
```
mongodump --db=kota_db --out=kota_db_dump
```
mongodump does not dump the content of the local database.
Do not use mongodump with db.fsyncLock().
mongodump overwrites output files if they exist in the backup data folder.

* (automongobackup.sh](https://raw.githubusercontent.com/micahwedemeyer/automongobackup/master/src/automongobackup.sh) ile

    - bulundugu dizin: /media/sas2tb/ipam/mongo/scripts/automongobackup.sh
    - Hata: mongodump failed to create dumpfile: oplog'u disable et
    - Hata: LC_TIME tr oldugundan turkce karakterli dosya yaratmaya calisiyor.
```
echo "export LC_TIME=en_US.UTF-8" >> /etc/profile
source /etc/profile
```

#### Araclar

* [Mongo-shell](http://docs.mongodb.org/manual/reference/mongo-shell/) kulllanim
 

