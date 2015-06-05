#### Graylog'u baslatma;

```
echo "export ES_HEAP_SIZE=2g" >> /etc/bash_profile
source /etc/profile
cd ~/graylog-setup
```

* Graylog baslatilir
```
nohup java -Xms1g -Xmx1g -XX:PermSize=128m -XX:MaxPermSize=256m -server \
-XX:+ResizeTLAB -XX:+UseConcMarkSweepGC -XX:+CMSConcurrentMTEnabled \
-XX:+CMSClassUnloadingEnabled -XX:+UseParNewGC -XX:-OmitStackTraceInFastThrow \
-Djava.net.preferIPv4Stack=true -Djava.library.path=lib/sigar -jar \
graylog/graylog.jar server -p run/graylog.pid -f conf/graylog.conf &
```

* elasticsearch baslatilir
```
nohup elasticsearch/bin/elasticsearch -f -p run/elasticsearch.pid \
-Des.config=conf/elasticsearch.yml &
```

* Web interface baslatilir
```
nohup graylog-web-interface/bin/graylog-web-interface -J-Xms128M -J-Xmx512m \
-Dhttp.port=10100 -Dpidfile.path=run/graylog-web-interface.pid \
-Dconfig.file=conf/graylog-web-interface.conf &
```
