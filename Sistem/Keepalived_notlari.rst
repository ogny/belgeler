=================
Keepalived
=================

:date: Pzt 16 Şub 2015 16:52:40 EET
:comments: true
:categories: 
:tags: 
:Author: Orkun Gunay


Workaround
-----------

Hata: VRRP Error : VRID not valid ! must be between 1 & 255. reconfigure !
VRRP_Instance the virtual id must be set

Hatanin chef deployment'ta olustuguyla karsilastim, 
`<https://github.com/rcbops/chef-cookbooks/issues/374>`_

#. Tum log'lari /var/log/messages'ta tutuyor.




Calisilan IP Listesi
--------------------

#. Floating: 195.175.249.107

#. mq-1: 195.175.249.105

#. mq-2: 195.175.249.106



Kaynaklar:
---------

#. `<http://thejimmahknows.com/load-balancing-and-failover-with-keepalived/>`_


