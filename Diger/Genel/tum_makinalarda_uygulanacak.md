 * Varolmayan repo dosyalarini kaldir.
```
mv /etc/yum.repos.d/CentOS-* ~/ 
cat << EOF > /etc/yum.repos.d/base.repo
[base]
name=  Base
baseurl=ftp://<IP>/base/
gpgcheck=0
enabled=1
EOF

cat << EOF > /etc/yum.repos.d/updates.repo
[updates]
name=  updates
baseurl=ftp://<IP>/updates/
gpgcheck=0
enabled=1
EOF

cat << EOF > /etc/yum.repos.d/sensu.repo
[sensu]
name=  Sensu
baseurl=ftp://<IP>/sensu/packages/
gpgcheck=0
enabled=1
EOF

cat << EOF > /etc/yum.repos.d/epel.repo
[epel]
name=  Epel
baseurl=ftp://<IP>/epel
gpgcheck=0
enabled=1
EOF

cat << EOF > /etc/yum.repos.d/nginx.repo
[nginx]
name=  nginx
baseurl=ftp://<IP>/nginx
gpgcheck=0
enabled=1
EOF

cat << EOF > /etc/yum.repos.d/pgdg95.repo
[pgdg95]
name=  PG
baseurl=ftp://<IP>/pgdg95/packages
gpgcheck=0
enabled=1
EOF

cat << EOF > /etc/yum.repos.d/scl.repo
[scl]
name=  Python
baseurl=ftp://<IP>/scl/packages
gpgcheck=0
enabled=1
EOF

```
* Not: sensu repo calisiyor mu, emin degilim, calismazsa asagidaki gibi elle
  paketi kuruyorum.
  
* kurulacak paketler
```
yum install -y screen ntp rsync sensu bc unzip
rsync -avh <IP>:~egem/repo_files/files/sensu/sensu-community-plugins ~/
sudo mv sensu-community-plugins /opt/sensu/
sudo chown sensu: /opt/sensu/ -R
```

* hostname tanimlama
```
vi /etc/sysconfig/network
HOSTNAME=
vi /etc/hosts
127.0.0.1 localhost hostname
hostname <hostname>
```

* iptables izinleri duzenlenir
```
chkconfig nfslock off 
chkconfig rpcbind off 
# Gerekli izinler tanimlanir tum gelen istekleri reject eden kuraldan once
# tanimlanir.
vi /etc/sysconfig/iptables
-A INPUT -m state --state NEW -m tcp -p tcp --dport 25 -j ACCEPT
-A INPUT -m state --state NEW -m udp -p udp --dport 123 -j ACCEPT
iptables -D INPUT -j REJECT --reject-with icmp-host-prohibited
service iptables start
chkconfig iptables on
```

* selinux disable
```
vi /etc/sysconfig/selinux
disabled
setenforce 0
```

* Sunucuya herhangi bir servis kurulduysa acilista baslat
```
chkconfig service on
```

* file ve process limitlerini arttirma
```
ulimit -n 63536
vi /etc/security/limits.d/90-nproc.conf
*                soft     nproc          63536
dpic       soft    nproc     63536 
vi /etc/security/limits.conf
*                -       nofile          63536
dpic - memlock unlimited
dpic - nproc 63536

echo "fs.file-max = 65536" >> /etc/sysctl.conf
sysctl -p
```

* Sunucu saatini eslestirme
```
chkconfig ntpd on
vi /etc/ntp.conf
server <IP>
(diger server'lari comment et)
/etc/init.d/ntpd start
```


```
cat << EOF > /etc/sensu/conf.d/client.json
{
  "client": {
    "name": "<hostname>",
    "address": "<IP>",
    "subscriptions": [
      "all",
      ""
    ],
    "keepalive": {
      "thresholds": {
        "warning": 21,
        "critical": 25 
      }
    },
    "handlers": [
      "email"
    ]
  }
}
EOF

cat <<EOF> /etc/sensu/config.json
{
    "rabbitmq": {
            "port": 5672,
            "host": "<IP>",
            "user": "sensu",
            "password": "sensu",
            "vhost": "/sensu"
    },
    "redis": {
            "host": "<IP>",
            "port": 6379
    },
    "api": {
            "host": "localhost",
            "bind": "0.0.0.0",
            "port": 4567
    },
    "dashboard": {
            "host": "<IP>",
            "port": 3000,
            "user": "admin",
            "password": "secret"
    }
}
EOF
```

* fstab'a noatime ekle. 
http://tldp.org/LDP/solrhe/Securing-Optimizing-Linux-RH-Edition-v1.3/chap6sec73.html
vi /etc/fstab
mount -a

* bashrc'ye ekle
cat << EOF >> /root/.bashrc
```
export HISTSIZE=10000
shopt -s histappend
export HISTTIMEFORMAT='%F %T '
export HISTCONTROL=ignoredups
EOF
```

echo << EOF >> /etc/rc.local
/usr/local/bin/python /usr/local/bin/supervisord -c /etc/supervisord.conf
EOF

vi /etc/rsyslog.conf
$ModLoad imudp
*.info;mail.none;authpriv.none;cron.none;keepalived.none                /var/log/messages
