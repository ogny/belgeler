* gzip kullanilip kullanilmadigini anlamak icin;
```
curl -I -H 'Accept-Encoding: gzip,deflate' http://site/
```

* curl -LSso:
`-LI: yonlendirme varsa arka tarafin header ciktisini verir.` 
`-o: ciktiyi belirtilen dosyaya yazar`
`-s: silent mode`
`-S: hatalari goster`

* Bir dosyadaki url'leri toplu download etme
```
xargs -n 1 curl -O < urls.txt
```
* kullanilmisbir curl, ne oldugu incelenecek
```
curl -o /dev/null -s -w %{time_connect}:%{time_starttransfer}:%{time_total}\\n http://domain.tld
```

* tek dosya indirme;

```
curl -O <url_adresi>
```


```
curl -H 'Content-Type: application/json' -H 'Accept: application/json' -X PUT \
'http://<ip>:<port>/api/<uri>' -d '{"by": "orkun"}'
```
