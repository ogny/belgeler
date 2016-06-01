### Sensu Monitoring framework


* Prod ve staging sunucularini ayri ayri takip edebilmek icin 2 sensu-api
  calistirilir.
    
* The Sensu architecture flow in a nutshell:
`Client` -> Subscription <- Check -> Event -> Filter -> Mutator -> Handler
`Client`: A client is a server running a check. Usually this is either a server
you are monitoring or a server monitoring another server/service.
`Subscription: Clients and checks are matched together through a subscription.
This can be fairly analogous to a template in other monitoring software since
you decide which check to run in a subscription and then you subscribe clients
to it.`
`Check`: A check, for checking things like resource usage or anything you can
think of.
`Event`: An event is generated every time a check is done. It’s the result of
the check, positive or negative, there’s always going to be one.
`Filter`: A filter is a way to decide which event is going to be triggering
notifications (handler execution).
`Mutator`: A mutator is run after a filter and modifies the event data. This
can be useful if your handler expects additional information not usually found
in the event.
`Handler`: A handler is actually anything that receives an event data and does
something with it. This could be an e-mail notification or a script that
restarts things. Anything.



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

