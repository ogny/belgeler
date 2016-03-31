* Sudo hakki olmayan kullanicinin supervisor ile servislerini yonetmesi
```
sudo groupadd supervisor
sudo usermod -a -G supervisor <kullanici_adi>
sudo vi /etc/supervisord.conf 
[unix_http_server]
file=/var/run/supervisor.sock   ; (the path to the socket file)
chmod=0770                       ; socket file mode (default 0700)
chown=root:supervisor
files = /etc/supervisor.d/*.conf , /home/<kullanici_adi>/supervisord_conf/*.conf

mkdir /home/<kullanici_adi>/supervisord_conf
cp  /etc/supervisor.d/*.conf /home/<kullanici_adi>/supervisord_conf/
supervisorctl stop all
supervisorctl reload 
su - <kullanici_adi>
supervisorctl start all
```

Not: socket'in veya pid sorun cikartirsa /tmp altina alabilirsin
