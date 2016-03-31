bigdesk de var yapmışken onu da alabilriiz
cluster.name: test
http.compression: true
config altinda yml var onu edit ediyoruz
node.name: "test1"
indices.fielddata.cache.size : 16GB
indices.fielddata.cache.expire : 720m
index.cache.filter.expire : 720m
script.disable_dynamic: false
bir de bin altinda elasticsearch dosyasina
JAVA_OPTS="$JAVA_OPTS -XX:+PrintGCDetails"
JAVA_OPTS="$JAVA_OPTS -XX:+PrintGCTimeStamps"
JAVA_OPTS="$JAVA_OPTS -XX:+PrintClassHistogram"
JAVA_OPTS="$JAVA_OPTS -XX:+PrintTenuringDistribution"
JAVA_OPTS="$JAVA_OPTS -XX:+PrintGCApplicationStoppedTime"
JAVA_OPTS="$JAVA_OPTS -Xloggc:/home/test/es/logs/gc.log"
ES_HEAP_SIZE=65536m
