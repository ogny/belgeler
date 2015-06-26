#### Kurulum

##### ubuntu icin, indirdiginde kurulum icin gerekenleri terminale dokuyor;

```
curl -SL https://get.docker.com/ubuntu/ 
```

##### Centos (epel repodan)
```
rpm -iUvh
http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
yum update -y
yum install docker-io
```
kurulan paket ve bagimliliklar
```
docker-io.x86_64 0:1.5.0-1.el6 
libcgroup
lua-alt-getopt
lua-filesystem
lua-lxc
lxc
lxc-libs          
xz
```

* Kurulabilecek diger paketler: (incelenecek)
```
docker-1.5-5.el6.x86_64. 
docker-io-devel-1.5.0-1.el6.x86_64. 
docker-io-logrotate-1.5.0-1.el6.x86_64. 
docker-io-vim-1.5.0-1.el6.x86_64. 
docker-io-zsh-completion-1.5.0-1.el6.x86_64. 
docker-registry-0.9.0-1.el6.noarch.
```

#### Temeller


* docker hostname'i Container id'den aliyor.
* docker history ile en guncel versiyonu bul. Not: (imaj adi images ciktisinda
  repository altinda)
* docker git branch gibi, temel alip yeni imaj olusturuyorsun, ornegin;
```
docker run -it --name="mycentos" centos7/mongo:latest /bin/bash 
```


```
docker images
docker history <REPOSITORY:TAG>
```
* IMAGE ID: imajin temel kimligi, diger hicbir sey olmasa da bununla is
  gorebiliyorsun.
* CONTAINER ID: pid gibi, son yapilan isin id'si

* Yapilan bir degisikligi host'ta container'in adiyla veya Container id'siyle
  gorebilirsin.
```
docker diff <name>
```
* container'da yaptigin degisikligi commit ettiginde yeni bir image
  olusturuyor, artik bu imajdan devam edebilirsin, yeni imajdan diledigimiz
  kadar olusturabiliriz.
```
docker commit -m "foo" <MEVCUT Container id veya name> \
<YENI Container id veya name>
```

#### Remove all untagged containers.

```
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
```

##### Docker yonetim komutlari

* docker da bir process oldugundan, istediginde kill et
```
docker kill 
```



* Ubuntu docker info ciktisinda;
Storage Driver: aufs
Root Dir: /var/lib/docker/aufs
Backing Filesystem: extfs

* Centos
Storage Driver: devicemapper


###### docker-run  parametreleri

* -entrypoint Dockerfile'daki parametreleri override eder 

* --name="" kullanilmazsa daemon bir random string atiyor. amac link'leri
  anlayabilmek

* -i ile container attach edilmese de tty'de acilir.



###### Kisayollar

* Container'i detach etme CTRL-P CTRL-Q.

Not: kisayollar tmux icinde screen'de de calisiyor.

###### aufs notlari;


[Genis kapsamli cheat sheat](https://github.com/wsargent/docker-cheat-sheet)

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

### Diger Bilgiler
Docker imajlarinin tutuldugu path: /var/lib/docker/containers

* container'lara ozgu donanim limitleri atamak icin cgroups'u kullaniyor.

* Here's a typical Docker build process:
```
FROM ubuntu:12.04
RUN apt-get update && apt-get install -y python python-pip curl
RUN curl -sSL https://github.com/shykes/helloflask/archive/master.tar.gz | tar
-xzv
RUN cd helloflask-master && pip install -r requirements.txt
```
[Kaynak:](https://github.com/docker/docker)

image ID ile container'i yonetiyorsun, 
container id ile commit ediyorsun ve yeni bir container id ediniyorsun.
(commit'ten sonra container id degismiyor, yeni image id olusuyor.)

* Docker'daki hazir imajlara guvenme, kendi imajini lxc'deki imajdan 
kendin olustur (not: systemd de ekle)

* sudo docker ps -l ile calisan container'i gorebiliyorsun (-l mutlak kullan)

##### Sorular


* nginx ve php-fpm'i daemonize olarak baslatma, docker file servisi yaziyor. bu
durum systemd icin gerekli mi?
* docker attach ile calisan container'e  baglanamadim, onun yerine run
kullaniyorum
```
docker run -i -t image_id /bin/bash

* docker only supports amd64

```
( docker run ile apt update upgrade yapamadim, dockerfile'a yazip deniyorum)

[zsh completion
plugin:](https://github.com/AeonAxan/oh-my-zsh/blob/master/plugins/docker/_docker)
* Bu plugin'i kullanabilmek icin docker.io binary'sini docker'a linkle
```
sudo ln -s /usr/bin/docker /usr/bin/docker.io
```

[sudo'suz kulllanma:](http://www.ludeke.net/2013/12/run-docker-commands-without-sudo.html)
```
sudo groupadd docker
sudo gpasswd -a ${USERNAME} docker
sudo service docker restart
```

* container'e girmeden ip adresini ogrenme
Run'la da olur inspect'le de, her ikisini de gorelim;
```
docker inspect -f '{{ .NetworkSettings.IPAddress }}' imaj_adi
```

##### Centos7 icin;

```
yum install deltarpm
yum --setopt=deltarpm=0  install <paket_adi>
```

* Centos'ta tum log'lar /var/log/docker altinda


#### Calisma Ortami:

NOT: vmware'deki makinada 2 docker image'i da internete cikamadi, yerel ubuntu
host'umdan cikti, docker0 interface'i ayni ip blogunu dagitiyor.
route olmadigi icin bakamadim

* kendi olusturdugum docker container'ini goremiyorum ;)
```
ps -l  vermiyor
images ciktisinda goruyorsun
```

* Containter'i stop ile durdurmak, icinde calisan process'i oldurmuyor, tekrar
  run dediginde process calismaya basliyor.

#### Kurulum

```
yum update -y
yum install docker-io docker-io-vim-1.5.0-1.el6.x86_64.rpm 
sudo docker search centos
sudo docker pull centos
```

#### Konteyner olusturma (manuel)

```
sudo docker run -ti centos /bin/bash
yum install -y screen vim-enhanced
```

* Host makinada 

```
sudo docker commit <id> mycentos/base
sudo docker run -ti mycentos/base /bin/bash
```

**Not**: container'in id'si prompt'ta mevcut.

#### Calisilacak

* Konteyner olusturma (Chef'le)
* do'daki debian'da calis

* http://www.jamescoyle.net/how-to/1531-using-dockerfiles-to-build-new-docker-images
* [Get Started with Docker Formatted Container Images on Red Hat Systems](https://access.redhat.com/articles/881893)
* https://github.com/veggiemonk/awesome-docker
* https://www.debian-administration.org/article/696/A_brief_introduction_to_using_docker
* https://docs.google.com/document/d/1Yb8pVDnibkipHw5lgftGkeN2s8oE0ex0QSl7aI4u89U/edit
* https://github.com/odewahn/docker-jumpstart/blob/master/public/docker-images.md

* http://nareshv.blogspot.com.tr/ 
#### Diger Bilgiler
* Docker imajlarinin tutuldugu path: /var/lib/docker/containers
* container'lara ozgu donanim limitleri atamak icin cgroups'u kullaniyor.
* container'e girmeden ip adresini ogrenme. Run'la da olur inspect'le de, her
  ikisini de gorelim;
```
docker inspect -f '{{ .NetworkSettings.IPAddress }}' imaj_adi
```

```
for file in *.tar; do eval `echo $file | \
sed -nr 's;([a-zA-Z0-9\.-]+)_([a-zA-Z0-9-]+)-([a-f0-9]+)\.tar;IMAGE=\1/\2\nVERSION=\3;pg'`; \
cat $file | docker import - ${IMAGE}:${VERSION}; done   
```
to import your containers, just make sure that there's only containers in the
directory where you run that line or it could get interesting :)


