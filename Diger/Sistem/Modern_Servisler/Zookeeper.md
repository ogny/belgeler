#### Indirme
```
curl -L -O http://ftp.itu.edu.tr/Mirror/Apache/zookeeper/zookeeper-3.4.8/zookeeper-3.4.8.tar.gz
tar zxvf zookeeper-3.4.8.tar.gz -C /home/dpic/apps/
ln -snf $HOME/zookeeper-3.4.8.t  $HOME/zk
./zk/bin/zkServer.sh start
```
