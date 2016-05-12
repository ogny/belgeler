#### kurulum
```
cd /usr/local/bin
curl -L https://github.com/kelseyhightower/confd/releases/download/v0.11.0/confd-0.11.0-linux-amd64 -o confd
chmod +x confd
## Template resource'un ve source template'in olusturulacagi dizinler
mkdir -p /etc/confd/{conf.d,templates}
```

* redis'te key value'lari tanimla
```
redis-cli set /myapp/database/url db.example.com
redis-cli set /myapp/database/user rob
```

* template'i duzenleyen config dosyasi
```
vi /etc/confd/conf.d/myconfig.toml
[template]
src = "myconfig.conf.tmpl"
dest = "/tmp/myconfig.conf"
keys = [
    "/myapp/database/url",
    "/myapp/database/user",
]
```
* config parametrelerini inceleyelim;
`src`: islenecek template dosyamizin yeri. 
uygulama default olarak /etc/confd/templates altinda ariyor
`dest`: template islenince olusan cikti. full path belirt
`keys`: backend'e path girilen key, islenen dest'te karsilik value giriyor.
(redis'te prefix belirtilebiliyor mu, test et)



* template dosyasi
```
vi /etc/confd/templates/myconfig.conf.tmpl
[myconfig]
database_url = {{getv "/myapp/database/url"}}
database_user = {{getv "/myapp/database/user"}}
```


* template'i isle



### Kaynaklar:
* [Reconfigure Services in CoreOS](https://www.digitalocean.com/community/tutorials/how-to-use-confd-and-etcd-to-dynamically-reconfigure-services-in-coreos)
* [kelseyhightower/confd](https://github.com/kelseyhightower/confd/blob/master/README.md)
