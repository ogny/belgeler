### Sensu Monitoring framework

* Prod ve staging sunucularini ayri ayri takip edebilmek icin 2 sensu-api
  calistirilir.
    


#### Kurulum

* sensu, epel, erlang repo'lari eklenir, 
```
yum install -y rabbitmq redis-server sensu uchiwa 
```

* Rabbitmq'da 2 user/vhost olusturulur
```
rabbitmqctl add_vhost /staging
rabbitmqctl set_permissions -p /staging sensu ".*" ".*" ".*"
```

* Redis'te multiple instances olusturulur ve calistirilir.

```
mv /etc/redis.conf{,.org}
cat << EOF > /etc/redis_6379.conf
port 6379
bind 0.0.0.0
EOF
cat << EOF > /etc/redis_6380.conf
port 6379
bind 0.0.0.0 
EOF
nohup /usr/sbin/redis-server /etc/redis_6379.conf  & 
nohup /usr/sbin/redis-server /etc/redis_6380.conf  & 
```
**NOT**: supervisord ile calistirma eklenecek

* Sensu API 2 farkli port'tan calistirilir
```
nohup /opt/sensu/bin/sensu-api  &
```



#### Notlar:

* herhangi bir client'ta config.json'la isin yok. o sensu-server icin

* checks/'e yeni eklenen json'larin calismama sebebi grubunun sensu
  olmamasiydi.

* sensu donus'lerde 2'ye (critical) mail donduruyor. ( bu konu biraz daha
  incelenecek)

* Sensu-server'i elle debug modda baslatmak istersen;
```
/opt/sensu/embedded/bin/ruby /opt/sensu/bin/sensu-server -b -c \
/etc/sensu/config.json -d /etc/sensu/conf.d -e /etc/sensu/extensions -p \
/var/run/sensu/sensu-server.pid -l /var/log/sensu/sensu-server.log -L debug
```

* sensu-community-plugins'te olmayan bir plugin'in calismasi icin, subscribe
  edilen sunuculara da yuklenmesi gerekiyor.

* metric'leri toplarken json dosyalarinda type:metric diye eklemeyi unutma

#### Redis

* redis monitoring icin client'lara redis gem'ini yukle
```
/opt/sensu/embedded/bin/gem install redis
```

* Handler'lar sensu-server'da /etc/sensu/conf.d/handlers/ altinda tanimlanir.
* As you may have realized, the Sensu check specification is compatible with
   Nagios, so you can use Nagios plugins with Sensu.


Kaynaklar
---------

* [official doc](http://sensuapp.org/docs/0.12/checks)

