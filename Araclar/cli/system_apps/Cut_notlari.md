* Belli bir karakterden sonrasini almak icin;
```
-c : karakter(N-)
```

* kolondan sonrasi icin;
```
-f : kolon(N-)
```

* log dosyasinda hata mesajlarinin en cok gecene gore siralanmasi
```
cut -d\  -f17- /var/log/php5-fpm.log |sort |uniq -c |sort -nr| head -n 10
cut -d\  -f9- /var/log/nginx-error-web.log |sort |uniq -c |sort -nr| head -n 10
```

* netstat'ta portu acik uygulamalari siralama;

```
netstat -anpt | grep apache2 |grep ESTABLISHED | awk '{ print $5 }' | cut -d: -f1 | sort -u
```

* sutunu alma; 
```
cut -d\  -f(sutun_no)-
```
