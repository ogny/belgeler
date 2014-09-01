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
cut -d\  -f11- /var/log/php5-fpm.log |sort |uniq -c |sort -nr| head -n 10
```
