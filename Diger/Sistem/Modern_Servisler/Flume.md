#### Upgrade
```
curl -L -O http://www.apache.org/dist/flume/1.6.0/apache-flume-1.6.0-bin.tar.gz
```
### Baska kullanicidan Tasima
```
su - hedef_kullanici
mv $HOME/apps/apache-flume-1.6.0-bin/conf/flume-conf.properties{,.org}
cp /home/<kaynak_kullanici>/flume/conf/flume-conf.properties $HOME/apps/apache-flume-1.6.0-bin/conf/
mv $HOME/apps/apache-flume-1.6.0-bin/conf/flume-env.sh.template{,.org}
cp $HOME/flume/conf/flume-env.sh.template $HOME/apps/apache-flume-1.6.0-bin/conf/
chown <hedef_kullanici>: $HOME/apps/apache-flume-1.6.0-bin -R
```

Baslatma
```
nohup $HOME/flume/bin/flume-ng agent -n <confta_gecen_agent> -c $HOME/flume/conf/ -f $HOME/flume/conf/<dosya.properties> &
```



