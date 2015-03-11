=================
Keepalived
=================

:date: Pzt 16 Şub 2015 16:52:40 EET
:comments: true
:categories: 
:tags: 
:Author: Orkun Gunay

Master dustugunde, backup'ta master_notify scriptini trigger etmemiz lazim, alternatifi
nedir? (notify_backup var mi?)

``Hata``:

VRRP Error : VRID not valid ! must be between 1 & 255. reconfigure !
VRRP_Instance the virtual id must be set

Hatanin chef deployment'ta olustuguyla karsilastim:: 

`<https://github.com/rcbops/chef-cookbooks/issues/374>`_

#. Tum log'lari /var/log/messages'ta tutuyor.


Virtual IP = 1
  195.175.249.105/32 dev eth0 scope global
Master state transition script = /var/lib/pgsql/scripts/promote.sh

VRRP Script = chk_postgres
  Command = killall -0 postgres
  Interval = 1 sec
  Timeout = 0 sec
  Weight = 2
  Rise = 1
  Fall = 1
  Status = INIT
Using LinkWatch kernel netlink reflector...
VRRP_Instance(VI_1) Entering BACKUP STATE
VRRP_Script(chk_postgres) succeeded


olusan sorunlar;
ip address associated with VRID not present in received packet one or more VIP
associated with VRID mismatch actual MASTER advert bogus VRRP packet received
on eth0 VRRP_Instance(VI_1) Dropping received VRRP packet

cozum: virtual_router_id'yi degistir (ip blogundan mi, ayni esxi'de
olmalarindan mi, yakaladi diger router_id 50'den calisan keepalived'yi)


Kaynaklar:
---------

#. `<http://thejimmahknows.com/load-balancing-and-failover-with-keepalived/>`_


