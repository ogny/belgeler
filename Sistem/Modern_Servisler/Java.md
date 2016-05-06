#### kurulum
```
mkdir /usr/java
tar zxvf jdk-8u73-linux-x64.tar.gz -C /usr/java
alternatives --install /usr/bin/java java /usr/java/jdk1.8.0_73/bin/java 1
echo "export JAVA_HOME=/usr/java/jdk1.8.0_73/bin" >> /etc/profile
source /etc/profile
```
* Daha onceden belirlenmis priority'i degistirmek icin
```
alternatives --config java
```

