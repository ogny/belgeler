* Platform: ubuntu 14.04 LTS x86_64 lxc 1.0.5
* kurulacak ek paketler debian-archive-keyring  uidmap
ubuntu'da network restart icin;
```
sudo service network-manager restart
```
Not: restart sonrasi br0 interface'inin varolup olmadigini gormek icin
/sbin/ifconfig ciktisina bak. interface yoksa makinayi reboot et.

* ubuntu lxc kurulumunda libvirt-bin gelmiyor, halbu ki cgroup-lite onun
  bagimliligi. Ancak kurduktan sonra  ubuntu'da lxc ile baglantili 2 interface cikiyor;
  10.0.3.0/24 dev lxcbr0  proto kernel  scope link  src 10.0.3.1 
  192.168.122.0/24 dev virbr0  proto kernel  scope link  src 192.168.122.1 
  ikisini de ben olusturmadim, bu konuyu aksam inceleyecegim.

### Kullanim
# Tum komutlar icin temel syntax;
* lxc-COMMAND --name=NAME -- COMMAND
### baslatma
lxc-start --logpriority=DEBUG --logfile=/tmp/test.log -n  test

### giris yapma
* lxc-attach -n test

#### SSH into it
* lxc-info -n {container-name}
ssh {container-name}@<ip from lxc-info>


### Kaynaklar:
* http://emrah.com/notlar/lxc_notlari.txt
* https://github.com/lxc/lxc/tree/master/templates
* #lxcontainers kanali
* http://www.polarsparc.com/xhtml/LXC.html

### Hata: 
```
sudo lxc-start -n {container_adi}
Failed to mount tmpfs at /dev/shm: Operation not permitted
```
