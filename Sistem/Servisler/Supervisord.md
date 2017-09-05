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
mv /etc/supervisor.d/*.conf  ~/supervisord_tasklar/
supervisorctl stop all
supervisorctl reload 
su - <kullanici_adi>
supervisorctl update all
supervisorctl start all
```

Not: socket'in veya pid sorun cikartirsa /tmp altina alabilirsin
[root@DSPTAPP02 conf.d]# supervisorctl
celery                           RUNNING   pid 22868, uptime 2 days, 1:35:13
celerybeat                       RUNNING   pid 17332, uptime 2 days, 2:27:15
dpicsmart-webapp                 RUNNING   pid 7920, uptime 3 days, 4:35:11
flume-kafka-conf                 STOPPED   May 23 03:16 PM
fmd                              STOPPED   May 26 03:04 PM
kafka                            RUNNING   pid 4547, uptime 13 days, 3:33:35
pmd-pmd2                         RUNNING   pid 19657, uptime 6:43:11
pmd_reshard                      STOPPED   May 23 04:32 PM
psnmp                            STOPPED   Jun 02 06:11 PM
storm-supervisor                 RUNNING   pid 4513, uptime 27 days, 0:37:40
storm-ui                         RUNNING   pid 37313, uptime 14 days, 1:04:55
ui.notification                  RUNNING   pid 31637, uptime 10 days, 0:31:10
ui.reporting                     RUNNING   pid 34883, uptime 3 days, 0:30:35
ui.tasks                         RUNNING   pid 11249, uptime 18 days, 18:18:12
wso2cep                          STOPPED   Not started
