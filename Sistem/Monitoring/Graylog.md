#### Graylog Kurulum (offline);

* Offline repo'da
```
mkdir /var/cache/yum/x86_64/6/graylog/
mkdir -p /var/cache/yum/x86_64/6/mongodb-org-3.0/packages/
```

* Uzak sunucuda:
```
curl -L -O \
https://packages.graylog2.org/repo/packages/graylog-1.1-repository-el6_latest.rpm
rpm -Uvh \
https://packages.graylog2.org/repo/packages/graylog-1.1-repository-el6_latest.rpm
yum install --downloadonly graylog-server graylog-web y
rsync -avh --progress /var/cache/yum/x86_64/6/graylog/ \
root@172.26.34.134:/var/cache/yum/x86_64/6/graylog/
yum install --downloadonly mongodb-org
rsync -avh --progress /var/cache/yum/x86_64/6/mongodb-org-3.0/packages/ \
root@172.26.34.134:/var/cache/yum/x86_64/6/mongodb-org-3.0/packages/
mkdir /opt/packages
cd /opt/packages
curl -L -O \
https://download.elastic.co/elasticsearch/elasticsearch/elasticsearch-1.7.1.noarch.rpm
rsync -avh --progress /opt/packages/ root@172.26.34.41:/opt/packages/
```

* Offline repo'da
```
rpm -ivh graylog-1.1-repository-el6_latest.rpm
cat << EOF > /etc/yum.repos.d/mongodb-org-3.0.repo
[mongodb-org-3.0]
name=Egemsoft pims MongoDB
baseurl=file:///var/cache/yum/$basearch/$releasever/mongodb-org
gpgcheck=0
enabled=1
EOF

cat << EOF > /etc/yum.repos.d/graylog.repo 
[graylog]
name=Egemsoft pims Graylog
baseurl=file:///var/cache/yum/$basearch/$releasever/graylog
gpgcheck=0
enabled=1
EOF
```


* Monitoring sunucusunda
    - selinux ve iptables kontrol edilir. Servisler aciksa kapatilir.
```
echo "fs.file-max = 65536" >> /etc/sysctl.conf
sysctl -p
sestatus
service iptables status
ulimit -n 63536
echo "export ES_HEAP_SIZE=2g" >> /etc/profile
source /etc/profile
rpm -ivh /opt/packages/elasticsearch-1.7.1.noarch.rpm
yum install -y java-1.7.0-openjdk
yum install -y mongodb-org
service mongod start
service elasticsearch start
yum install graylog-server graylog-web
cp /etc/graylog/server/server.conf{,.org}
cp /etc/elasticsearch/elasticsearch.yml{,.org}
service graylog-server start
service graylog-web start

cat << EOF > /etc/yum.repos.d/mongodb-org-3.0.repo
[mongodb-org-3.0]
name=Egemsoft pims MongoDB
baseurl=ftp://172.26.34.134/mongodb-org-3.0
gpgcheck=0
enabled=1
EOF

cat << EOF > /etc/yum.repos.d/graylog.repo 
[graylog]
name=Egemsoft pims Graylog
baseurl=ftp://172.26.34.134/graylog
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-graylog
enabled=1
EOF
```






Workaround:
cat /proc/sys/fs/file-max
cat /proc/sys/fs/nr_open
Make sure to install and configure Java (>= 7), MongoDB and Elasticsearch
before starting the Graylog services.



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


