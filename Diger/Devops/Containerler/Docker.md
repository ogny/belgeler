## TANIMLAR:

* Docker images: 

read-only template. konteyner olusturmakta kullaniliyor. Docker images are he
build component of Docker.

* Docker registries:
imajlarin yuklenip indirildigi alan. Docker registries are the distribution
component of Docker.

* Docker containers
Tipki bir dizinmis gibi dusun. calistirilacak her sey icinde. imajdan
olusturuluyor. Docker containers are the run component of Docker.

#### Buraya kadar ne ogrendik?

1. You can build Docker images that hold your applications.
2. You can create Docker containers from those Docker images to run your
   applications.
3. You can share those Docker images via
   [Docker Hub](https://hub.docker.com) or your own registry.

Docker images are then built from these base images using a simple, descriptive
set of steps we call *instructions*. Each instruction creates a new layer in our
image. Instructions include actions like:

* Run a command.
* Add a file or directory.
* Create an environment variable.
* What process to run when launching a container from this image.

These instructions are stored in a file called a `Dockerfile`. Docker reads this
`Dockerfile` when you request a build of an image, executes the instructions, and
returns a final image.

#### Temeller

* container sadece sen bir komut calistiginda aktif, degilse kapaniyor; run -it
  ile interaktif kullaniyoruz. Kapandiktan sonra, yapilan degisiklikler commit
  edilmediyse, ucuyor.
So what happened to our container after that? Well Docker containers
only run as long as the command you specify is active. Here, as soon as
`Hello world` was echoed, the container stopped.
* docker hostname'i Container id'den aliyor.
* docker history ile en guncel versiyonu bul. Not: (imaj adi images ciktisinda
  repository altinda)
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
* ps parametreleri;
  - `-l` son olusturulan.
  - `-a` simdiye kadar olusturulmus tum container'lar.

* bir docker imajindan istedigin sayida yeni docker container'i ayni anda aktif
  kullanabiliyorsun. bir nevi klonlamak gibi. (cluster vb. yapilar icin
  inanilmaz bir kolaylik)

* docker container'a login olurken image adini yaziyoruz, bunun disinda her
  seyi container_id ile yapiyoruz, 

* `docker image history`, image building'le ilgili bir olay. container tarafini
  baglamiyor.

* bir container'i kapattiginda `docker stop` , tekrar ihtiyac duyulursa, yeni
  bir container olusturmak yerine  `docker start takma_ad` ile tekrar
  calistirabilirsin.

* imaj olusturma (build): Yapi: INSTRUCTION arguments
  - DockerFile'a tanimlanan her instruction birbirinden bagimsiz calisir ve
    yeni bir imaj olusumuna sebep olur. `RUN cd /tmp` gibi bir instruction'un
    hicbir etkisi olmayacaktir.

  - ENV instruction'u variable'lar icin kullanabilirsin, kullanilabilir env. variable instruction'lari;
`ADD`
`COPY`
`ENV`
`EXPOSE`
`LABEL`
`USER`
`WORKDIR`
`VOLUME`
`STOPSIGNAL`

* Debugging;


#### sorunlar:
  konetyniri baslattigimda ftp server acilmiyor (chkconfig on)


#### Kurulum

* [Debian icin](https://docs.docker.com/engine/installation/debian/)

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

```bash
for file in *.tar; do eval `echo $file | \
sed -nr 's;([a-zA-Z0-9\.-]+)_([a-zA-Z0-9-]+)-([a-f0-9]+)\.tar;IMAGE=\1/\2\nVERSION=\3;pg'`; \
cat $file | docker import - ${IMAGE}:${VERSION}; done   
```
to import your containers, just make sure that there's only containers in the
directory where you run that line or it could get interesting :)

* centos7 docker'da  `pg_ctl status` ciktisi
```
pg_ctl: directory "/var/lib/pgsql/9.4/data" is not a database cluster directory
RUN echo '' >> /etc/sudoers && echo 'jenkins ALL=(ALL NOPASSWD: ALL' >> /etc/sudoers)'
```

### Registry repo komutlari

Nexus Repo Manager 3 ile private Docker registry repo özelliği geldi. 
Özetle; docker imajlarınızı upload/download edebiliyorsunuz.
Bu yazıda docker registry repo'yu lokal makinanıza nasıl tanıtacağınızdan bahsedilecek.

docker kurulumu icin: https://docs.docker.com/engine/installation/

sudo vi /etc/hosts
ip hostname

Ubuntu 16.04 icin;
docker.servisini başlatan ExecStart parametresi düzenlenir, servis yeniden başlatılır.

sudo vi /etc/systemd/system/docker.service 
ExecStart=/usr/bin/dockerd -H fd:// --insecure-registry "hostname:port"
sudo systemctl daemon-reload
sudo systemctl restart docker.service

Ubuntu 14.04 icin;
Test edilmedi

Debian Jessie;
ExecStart parametresi mevcut çalışan dosyadan silinip override oluşturalacak
yeni yapılandırma dosyasında belirtilir.
sudo vi /lib/systemd/system/docker.service
sudo mkdir  /etc/systemd/system/docker.service.d
sudo vi /etc/systemd/system/docker.service.d/docker.service.conf
[Service]
ExecStart=/usr/bin/docker daemon -H fd:// --insecure-registry "hostname:port"
sudo systemctl daemon-reload
sudo systemctl restart docker.service

#### centos yapilanlar
* [kaynak](https://docs.docker.com/engine/installation/linux/centos/)
```
vi /etc/hosts
10.134.50.229 registry.sekomy.com
systemctl enable docker.service
vi /usr/lib/systemd/system/docker.service
ExecStart=/usr/bin/dockerd  --insecure-registry registry.sekomy.com --insecure-registry registry.sekomyazilim.com.tr 
ln -s /usr/lib/systemd/system/docker.service /etc/systemd/system/ 
systemctl daemon-reload 
systemctl restart docker.service
docker info
docker login registry.sekomy.com
```
çıktıda Insecure Registries'te oluşturduğun ip'lerin görünecek

http://uadmin.nl/init/docker-share-data-between-containers/
1. Share a docker volume
- create the volume in a container that you do not run
docker create -v /webdata –name webd1 centos

- start a second container and use the volume from the 1st container
docker run -it -d –volumes-from webd1 –name webd2 centos

- attach to the container
docker attach `docker ps -a|grep webd2|awk ‘{print $1}’`

- put a file in /webdata
touch /webdata/afile

- exit the container
[root@6c6e823f6eb4 /]# exit

- start a third container and use the volume from the 1st container
docker run -it -d –volumes-from webd1 –name webd3 centos

-attach to the third container
docker attach `docker ps -a|grep webd3|awk ‘{print $1}’`

- list the contents of /webdata
ls /webdata
afile

- exit the container
[root@4531a52b8e2a /]# exit

- remove all three containers
docker rm -f `docker ps -a|grep webd*|awk ‘{print $1}’`

- remove the orphaned volume
docker volume ls -qf dangling=true | xargs -r docker volume rm

In the following example we create a directory on the host
and share the directory between containers.

2. share a host directory
- create a directory on the host
mkdir -p /docker/shared

- create a container and use the directory
docker run -d -it –name webd5 -v /docker/shared:/shared centos

-attach to the container
docker attach `docker ps -a|grep webd5|awk ‘{print $1}’`

- create file in /shared
touch /shared/afile

- exit the container and remove it.
[root@a4f1061d8d9f /]# exit
docker rm -f `docker ps -a|grep webd5|awk ‘{print $1}’`

- list the contents of /docker/shared
ls /docker/shared
afile

- start a new container with the shared directory
docker run -d -it –name webd6 -v /docker/shared:/shared centos

- attach to the container
docker attach `docker ps -a|grep webd6|awk ‘{print $1}’`

- create a file in /shared
touch /shared/bfile

- list the contents of /shared
ls /shared
afile bfile

- exit the container and check /docker/shared
[root@f4ba93a953c3 /]# exit
ls /docker/shared
afile bfile

Yöntem:
önce imajdan bir container oluşturuyoruz, sonra onu bir servis gibi açıp
kapıyıp silip yeniden oluşturabiliyoruz.

* host'tan dosya kopyalamak
`docker cp foo.txt mycontainer:/foo.txt`

* container'dan komut çalıştırma 
`docker run ubuntu /bin/echo 'Hello world'`

* daemon (detached) modda çalıştırmak icin `-d` 
  bu moddan çıkmak için: `docker stop`

* farkli bir config ile çalıştırmak icin `--config <dizin>`


Ornek:
docker run -d -p 9000:9000 portainer/portainer
docker ps -a
CONTAINER ID        IMAGE                 COMMAND             CREATED             STATUS                      PORTS                    NAMES
a5c3fa760bc         centos6-pg94          "/bin/bash"          10 minutes ago      Exited (0) 10 minutes ago                            focused_roentgen
eece181300d4        portainer/portainer   "/portainer"        About an hour ago   Up About an hour            0.0.0.0:9000->9000/tcp   festive_joliot
docker run -it centos6-pg94 bash

* Dosya kopyalama
```
docker cp <containerId>:/file/path/within/container /host/path/target
```
#### Dockerfile notlari;
* RUN: build ederken tanimli komutu calistiriyor.
2.kez build ettiginde bu asamayi geciyor;
```
Step 2/3 : RUN apt-get -y update
 ---> Using cache
 ---> 8bbc00346b46
```
* CMD duz olarak calistirildinda tanimli komutlari calistiriyor, burada ansible
  playbook devreye girebilir. fakat container'in icine nasil kuracagiz, o iste
  kitchen calismalarindan devsirilebilir.

* EXEC (Dockerfile'da gecmiyor)
Run a command in a running container

* Note that each instruction is run independently, and causes a new image to be
  created - so RUN cd /tmp will not have any effect on the next instructions.

#### Dockerfile Ansible karsiliklari;

* FROM \*must\* be the first instruction in the Dockerfile.
* LABEL (meta/main.yml )
* ENV = inventory this instruction allows one to set and persist
  environment variables in the resulting Docker image. Instructions following
  this can reference the set environment variables if needed.
* ADD = files/ kopyalar.
* EXPOSE docker'daki uygulama hangi portu dinleyecek
* ENTRYPOINT imaj baslatildiginda container'da son olarak hangi uygulama calisacak
  syntax: ENTRYPOINT ["<executable>", "<arg-1>", "<arg-2>", ..., "<arg-n>"]
  Or: ENTRYPOINT ["python", "/home/flask/app.py"]
* WORKDIR RUN, ADD, or ENTRYPOINT'in calisacagi dizini belirt. 
  syntax: WORKDIR <path> (full path)



#### Services in containers: <service> dead but pid file exists
Images don't preserve running processes. Images are essentially a snapshot of
the filesystem. Thus you're starting something, the PID file is created, and
then you create a snapshot of the filesystem. So when you create a container
based on that snapshot, there is no process matching the PID file.

* Build'te birden cok tag ver ve dockerfile path'i goster
```
docker build -f tests/Dockerfile-centos6 -t ansx/centos6_pg95:v1 -t ansx/centos6_pg95:latest .
```

```
run  herseferinde yeni bir container başlatir, gecici, denemelik bi is icin kullan,
exec ile calisan container'a attach olursun 
docker exec -it prod_postgresql_1 bash 
```

#### cheat sheet
```
docker build -t friendlyname .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyname  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyname         # Same thing, but in detached mode
docker ps                                 # See a list of all running containers
docker stop <hash>                     # Gracefully stop the specified container
docker ps -a           # See a list of all containers, even the ones not running
docker kill <hash>                   # Force shutdown of the specified container
docker rm <hash>              # Remove the specified container from this machine
docker rm $(docker ps -a -q)           # Remove all containers from this machine
docker images -a                               # Show all images on this machine
docker rmi <imagename>            # Remove the specified image from this machine
docker rmi $(docker images -q)             # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry
```

#### docker-machine
virtualbox'ta boot2docker ile imaj olusturuyor.

* imajlari ve tag'larini alfabetik listeleme;
```
docker image ls |awk '{print " "$1":"$2" "}' |sort -n 
```

* detach olamadigin durumlar yasanmamasi icin;
```
--sig-proxy=false
```

### networking

* User-defined networks
It is recommended to use user-defined bridge networks to control which
containers can communicate with each other, and also to enable automatic DNS
resolution of container names to IP addresses. 
You can create a new bridge network, *overlay* network or *MACVLAN* network. You
can also create a network plugin or remote network for complete customization
and control.
Within a user-defined bridge network, linking is not supported. You can expose
and publish container ports on containers in this network. This is useful if
you want to make a portion of the bridge network available to an outside
network
A bridge network is useful in cases where you want to run a relatively small
network on a single host. You can, however, create significantly larger
networks by creating an overlay network.

#### docker-machine
```
curl -L https://github.com/docker/machine/releases/download/v0.11.0/docker-machine-`uname -s`-`uname -m` \
>/tmp/docker-machine &&  chmod +x /tmp/docker-machine && \
sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
```

#### How do I change the Docker image installation directory 
/etc/default/docker file with the -g option: DOCKER_OPTS="-dns 8.8.8.8 -dns 8.8.4.4 -g /mnt"

* `dokcer ps`  ciktisinda container id &  container name'i gorme
docker ps |awk '{print " "$1"     "$NF" "}' |sort -n

* Note: One advantage of using nsenter to run commands in a pod's namespace –
  versus using something like docker exec – is that you have access to all of
  the commands available on the node, instead of the typically limited set of
  commands installed in containers.
```
docker ps
docker inspect --format '{{ .State.Pid }}' container-id-or-name
nsenter -t your-container-pid -n ip addr
```
