### Tutorial'dan notlar;
calisilan ortam;
Versiyonlar:
client ve server 0.5.3

* Kullanabilecegin imajlar;
```
docker images
```
* bir tanesini secip cek
```
docker pull learn/tutorial
```

* cektiginde bir komut calistir;
```
docker run learn/tutorial echo "hello world"
```
docker run learn/tutorial apt-get install -y ping
```

* Program kurma;

* Herhangi bir program kurduktan sonra onu kullanmak icin git'teki gibi
versiyonlaman, yani commit etmen gerek. syntax'i soyle;
```
docker commit ID subject/yeni_uygulama;
docker commit ID learn/ping
```

* yeni imaj'dan program calistiralim
```
docker run learn/tutorial ping google.com
```

* tekrar docker ps yaptiginda ping calistirdigin versiyonun degistigini
goruyorsun;

* docker inspect ciktisi json olarak cnotainer bilgisini iceriyor;
* docker push subject/komut ile  docker havuzuna imajini atabiliyorsun.


nginx ve php-fpm'i daemonize olarak baslatma, docker file servisi yaziyor. bu
durum systemd icin gerekli mi?


