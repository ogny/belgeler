Redis cluster
=============

Cluster komutlari
~~~~~~~~~~~~~~~~~

::

    redis-trib.rb del-node     
    redis-trib.rb check

#. Yapilacak: cluster docker images'ta calistirilip test edilecek.



Arastirma:
~~~~~~~~~~

cluster'daki inconsistency


* Redis Cluster does not use consistency hashing, but a different form of
  sharding where every key is conceptually part of what we call an hash slot.

* Yeni not eklemek istedigimizde, diger node'lara tanimladigimiz hash slot'lari
  yeni node'a kaydiriyoruz. boylelikle operasyonel duzeyde islem yapmadan
  (durdurma vb.) node ekleyip cikartabiliyoruz.

* The user can force multiple keys to be part of the same hash slot by using a
  concept called hash tags.

* Redis Cluster specification: if there is a substring between {} brackets in a
  key, only what is inside the string is hashed. Bu durumda; bu{foo}key ile
  diger{foo}key 'in ayni hash slot icerisinde oldugu garanti altindadir.

YAPI
----

* Her VM icerisinde bir master bir diger master'in slave node'u barindirilsin.
  A -> B1 gibi. Bu durumda hem node'lardan biri veya VM'in erisilemez oldugu
  durumda cluster bozulmadan calismaya devam edebilir.

* Node B1 replicates B, and B fails, the cluster will promote node B1 as the
  new master and will continue to operate correctly.  However note that if
  nodes B and B1 fail at the same time Redis Cluster is not able to continue to
  operate.




    port'lar default olsun


    yum groupinstall -y "Development tools"
    doc'taki default kurulum adimlari
    cd /usr/local/src
    wget http://download.redis.io/releases/redis-3.0.0.tar.gz
    tar 
    make 

    ulimit set

    orjinali kopyalayip gonder
    vi /etc/redis.conf
    port 7000
    cluster-enabled yes
    cluster-config-file nodes.conf
    cluster-node-timeout 5000
    appendonly yes


    nohup ./redis-server /etc/redis1.conf &
    nohup ./redis-server /etc/redis2.conf &
    
    ./redis-cli -p 7000
        keys *
        cluster nodes
    
    
    yum install -y ruby rubygem rsync
    gem install redis
    ./redis-trib.rb create --replicas 1 

    cluster'in durumunu gormek icin tekrar;
        ./redis-cli -p 7000
         keys *

        test edelim
        git clone https://github.com/antirez/redis-rb-cluster.git
        cd redis-rb-cluster
        duzenle;
        cluster'da sadece master'lar var
    startup_nodes = [
        {:host => "127.0.0.1", :port => 6379},
        {:host => "127.0.0.1", :port => 6380}
        ruby example.rb


BELGE
~~~~~

* Yapilacaklar::

  Supervisord ile baslatilacak.

* Sistem Genel::

    ulimit -n 63536

    vi /etc/sysctl.conf
    fs.file-max = 65536
    net.core.somaxconn=65535
    vm.overcommit_memory=1

    echo never > /sys/kernel/mm/transparent_hugepage/enabled

    /etc/security/limits.d/90-nproc.conf
    * -     nproc       63536

    /etc/security/limits.conf
    * -     nofile      63536

    cd /usr/local/src/
    mkdir mycluster ; cd mycluster ; mkdir 6379 6380
    mv /usr/local/src/redis-3.0.0/redis.conf{,.org}

* Derleme islemleri ve config dosyalari tek sunucuda olusturulur, rsync'le
  digerlerine gonderilir.::

6379/redis.conf::

    cat << EOF > 6379/redis.conf
    port 6380
    cluster-enabled yes
    cluster-config-file nodes6380.conf
    cluster-node-timeout 5000
    appendonly yes
    EOF

6380/redis.conf::

    cat << EOF > 6380/redis.conf
    port 6380
    cluster-enabled yes
    cluster-config-file nodes6380.conf
    cluster-node-timeout 5000
    appendonly yes
    EOF

    rsync -avHPS ../ <diger_sunucular>:/usr/local/src/redis-3.0.0/src/

* Redis-server'i baslatma::

    nohup /usr/local/src/redis-3.0.0/src/redis-server \
    /usr/local/src/mycluster/6379/redis.conf &
    nohup /usr/local/src/redis-3.0.0/src/redis-server \
    /usr/local/src/mycluster/6380/redis.conf &

* Replication'u baslatma::

    cd /usr/local/src/redis-3.0.0/src
    ./redis-trib.rb create --replicas 1 sunucu_1:6379  \
    sunucu_1:6380 sunucu_2:6379 sunucu_2:6380  \
    sunucu_3:6379 sunucu_3:6380


Hatalar
~~~~~~~

::
    HARD - SOFT RESET eklenecek

