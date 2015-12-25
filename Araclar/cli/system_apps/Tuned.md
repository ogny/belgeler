#### Paketler;

* tuned
* tuned-utils

#### Yapilandirma

* Calistirma
```
tuned-adm profile latency-performance
Applying ktune sysctl settings:
/etc/ktune.d/tunedadm.conf:                                [  OK  ]
Calling '/etc/ktune.d/tunedadm.sh start':                  [  OK  ]
Applying sysctl settings from /etc/sysctl.conf
Starting tuned:                                            [  OK  ]
tuned-adm active
Current active profile: latency-performance
Service tuned: enabled, running
Service ktune: enabled, running
```


#### Kaynaklar

http://wiki.mikejung.biz/OS_Tuning
https://eaeraybar.com/centos-7-tuning-profile-server
http://docs.mongodb.org/manual/reference/transparent-huge-pages



