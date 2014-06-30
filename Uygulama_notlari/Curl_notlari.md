* gzip kullanilip kullanilmadigini anlamak icin;
```
curl -I -H 'Accept-Encoding: gzip,deflate' http://site/
```

* curl -LSso:
`-LI: yonlendirme varsa arka tarafin header ciktisini verir.` 
`-o: ciktiyi belirtilen dosyaya yazar`
`-s: silent mode`
`-S: hatalari goster`

