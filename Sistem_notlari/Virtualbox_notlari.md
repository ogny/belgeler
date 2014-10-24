Sorun: shutdown komutu ile bilgisayar kapatilamiyor, sistem asagidaki log'u basiyor
```
kernel:[31089.065000] unregister_netdevice: waiting for vboxnet0 to become
free. Usage count = -1
Cozum:
```
/etc/default/virtualbox and change the line
SHUTDOWN_USERS=""
to
SHUTDOWN_USERS=all
[Kaynak:](https://www.virtualbox.org/ticket/12264)
