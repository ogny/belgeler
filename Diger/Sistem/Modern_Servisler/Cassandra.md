Cassandra Yonetimi
===================

* tutulan veri: queuebased -l2tp - kotanin datasi
* Cassandra fail oldugunda ilgili makinaya baglanip; 

```
service cassandra restart
su - cyaniteuser
nohup java -jar cyanite/target/cyanite-0.1.3-standalone.jar &
```

* Spark'i yeniden baslatmak icin ipam-ulus-db-2'ye baglanip;

```
/data/spark/sbin/stop-all.sh
/data/spark/sbin/start-all.sh
```
* servisin duzgun calisip calismadigi kontrol edili;

```
nodetool status
```
