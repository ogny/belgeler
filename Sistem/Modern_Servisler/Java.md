#### kurulum
```
curl -L -O -H "Cookie: oraclelicense=accept-securebackup-cookie" -k \
"http://download.oracle.com/otn-pub/java/jdk/8u92-b14/jdk-8u92-linux-x64.tar.gz"
mkdir /usr/java
tar zxvf jdk-8u92-linux-x64.tar.gz -C /usr/java
alternatives --install /usr/bin/java java /usr/java/jdk1.8.0_73/bin/java 1
echo "export JAVA_HOME=/usr/java/jdk1.8.0_92" >> /etc/profile
export PATH=/usr/java/jdk1.8.0_92/bin:$PATH
source /etc/profile
```
* Daha onceden belirlenmis priority'i degistirmek icin
```
alternatives --config java
```

