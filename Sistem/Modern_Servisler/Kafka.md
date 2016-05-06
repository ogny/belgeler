#### Indirme, Yapilandirma
```
curl -L -O http://ftp.itu.edu.tr/Mirror/Apache/kafka/0.9.0.1/kafka_2.11-0.9.0.1.tgz
tar zxvf kafka_2.11-0.9.0.1.tgz
ln -snf $HOME/kafka_2.11-0.9.0.1 $HOME/kafka
cp $HOME/kafka/config/server.properties{,.org}
vi $HOME/kafka/config/server.properties
log.dirs=
zookeeper.connect=:<IP>:2181
```

* Cluster yapida config dosyasinda unique `broker.id`'ler tanimlamak gerekiyor.
  Eger cluster ayni `broker.id` ile baslatildiysa ilk alan calisir, digerinin
  de calismasi icin `broker.id`'si degistirilen sunucuda
  `$HOME/kafka-logs/meta.properties`'te de broker.id degistirilmeldir.


#### Baslatma
```
nohup $HOME/kafka/bin/kafka-server-start.sh $HOME/kafka/config/server.properties 2>&1 &
```


