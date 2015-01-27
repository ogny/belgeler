====================
Virtualbox Case'leri
====================

:date: 2015-01-16

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

#. Guest'i VBoxheadless olarak baslatma: 
[Kaynak:](https://www.howtoforge.com/vboxheadless-running-virtual-machines-with-virtualbox-4.3-on-a-headless-ubuntu-14.04-lts-server)

#. Guest icin static ip atama;
[Kaynak:](http://coding4streetcred.com/blog/post/VirtualBox-Configuring-Static-IPs-for-VMs)

