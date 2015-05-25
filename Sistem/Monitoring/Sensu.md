### Sensu Monitoring framework

* herhangi bir client'ta config.json'la isin yok. o sensu-server icin
  olusturuluyor.

* checks/'e yeni eklenen json'larin calismama sebebi grubunun sensu
  olmamasiydi.

* sensu donus'lerde 2'ye (critical) mail donduruyor. ( bu konu biraz daha
  incelenecek)

#### Redis

* redis monitoring icin client'lara redis gem'ini yukle
```
/opt/sensu/embedded/bin/gem install redis
```

#. As you may have realized, the Sensu check specification is compatible with
   Nagios, so you can use Nagios plugins with Sensu.


Kaynaklar
---------

#. [official doc](http://sensuapp.org/docs/0.12/checks)

*
