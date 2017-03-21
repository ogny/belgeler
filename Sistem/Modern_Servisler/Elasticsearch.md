```
elasticsearch/config/elasticsearch.yml
node.name: ""
cluster.name: ""
http.compression: true
indices.fielddata.cache.size : 16GB
indices.fielddata.cache.expire : 720m
index.cache.filter.expire : 720m
script.disable_dynamic: false

elasticsearch/bin/elasticsearch.in.sh
JAVA_OPTS="$JAVA_OPTS -XX:+PrintGCDetails"
JAVA_OPTS="$JAVA_OPTS -XX:+PrintGCTimeStamps"
JAVA_OPTS="$JAVA_OPTS -XX:+PrintClassHistogram"
JAVA_OPTS="$JAVA_OPTS -XX:+PrintTenuringDistribution"
JAVA_OPTS="$JAVA_OPTS -XX:+PrintGCApplicationStoppedTime"
JAVA_OPTS="$JAVA_OPTS -Xloggc:/home/<kullanici>/es/logs/gc.log"
```

* baslatma ve check etme
```
nohup ./es/bin/elasticsearch 2>&1 & 
./es/bin/elasticsearch -d
jps
```

* Head plugin kurulumu eklenecek  (offline ortamda elle tasiyarak calistirabildik.)
* bashrc'ye eklenecek
```
ES_HEAP_SIZE=65536m
```

