* Gerekli paketler:
    ```
    yum install -y yum-utils createrepo
    ```
* mevcut repo'larin id'lerini ogrenmek icin
    ```
    yum repolist
    ```
* CentOS minimal'le gelen base, extras ve postgre-94 repo'lari clone'lanir.

* Diger repo'lardan cekilecek paketler tespit edilir.


### Kaynaklar:

* [How to create a local mirror of the latest update for Red Hat Enterprise Linux 5, 6, 7 without using Satellite server?](https://access.redhat.com/solutions/23016)


### Eklenecek repo'lar

```
erlang_solutions
pgdg
nginx
epel
sensu
```

### repo'lardan alinacak paketler arastirilir:

```
epel-release
libxslt-devel
libxml2-devel
postfix
java-1.8.0-openjdk
vsftpd
rsync
ruby
ntp
sensu
keepalive
repmgr
groupinstall "development tools"
net-tools
wget
python-devel
python-setuptools
git
screen
```

### pip ile kurulanlar

```
virtualenv
supervisor
celery
beat
gunicorn
django
psycopg2
```

### manuel indirilecek 

```
curl -L -O
https://web-dl.packagecloud.io/chef/stable/packages/el/6/chef-server-core-12.0.7-1.el6.x86_64.rpm
curl -L -O https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py
curl -L -O -H "Cookie: oraclelicense=accept-securebackup-cookie" -k \
"https://edelivery.oracle.com/otn-pub/java/jdk/8u40-b25/jdk-8u40-linux-x64.rpm"
curl -L -O http://yum.postgresql.org/9.4/redhat/rhel-6-x86_64/pgdg-centos94-9.4-1.noarch.rpm
curl -L -O http://download.redis.io/releases/redis-3.0.0.tar.gz
curl -L -O
http://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm
curl -L -O
http://www.rabbitmq.com/releases/rabbitmq-server/current/rabbitmq-server-3.5.1-1.noarch.rpm
curl -L -O http://nginx.org/download/nginx-1.6.3.tar.gz
```

redis 3.0
python 2.7.2
```

### Sorulacaklar

* jasper (pims'te var mi)
