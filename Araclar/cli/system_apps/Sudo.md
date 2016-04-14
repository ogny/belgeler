* oturumu olmayan kullaniciya oturum actirtma;
```
sudo su - nginx -s /bin/bash
```

* 1024'ten dusuk port'lari kullaniciyla calistirma
```
visudo  
Defaults    !requiretty             # TTY mecburiyeti kaldırılır.
nginx ALL=NOPASSWD: /usr/sbin/nginx # kullanıcıya uygulama çalıştırma izni verilir.
:wq
su - nginx -s /bin/bash -c "sudo  /usr/sbin/nginx -c /etc/nginx/nginx.conf" 
```

