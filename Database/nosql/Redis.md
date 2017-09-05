### Test amacli 

* Cluster olusturma;
``` bash
docker pull redis
sudo docker run -ti redis:latest /bin/bash 
apt-get install -y vim-tiny
curl -L -O http://download.redis.io/redis-stable/src/redis-trib.rb
chmod +x redis-trib.rb
mv redis-trib.rb /usr/local/bin
mkdir 1270{1,2,3,4,5,6,7}
for port in 1270{1..6}; do ( cd $port && redis-server \
--cluster-enabled yes --daemonize yes --port $port ); done
redis-trib.rb create --replicas 1 127.0.0.1:1270{1..6}
```
* failover calisiyor mu bir master durdurulur
`redis-cli -p 12702 debug segfault`

