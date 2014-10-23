
Platform: ubuntu 14.04 LTS x86_64
(kaynak: emrah.com lxc_notlari)
* kurulacak ek paketler debian-archive-keyring 
ubuntu'da network restart icin;
sudo service network-manager restart

komutlar;
lxc
lxc-checkconfig
lxc-create
lxc-halt
lxc-ls
lxc-restart
lxc-shutdown
lxc-unfreeze
lxc-attach
lxc-checkpoint
lxc-destroy
lxc-info
lxc-monitor
lxc-restore
lxc-start
lxc-unshare
lxc-backup
lxc-clone
lxc-execute
lxc-kill
lxc-netstat
lxc-setcap
lxc-stop
lxc-version
lxc-cgroup
lxc-console
lxc-freeze
lxc-list
lxc-ps
lxc-setuid
lxctl
lxc-wait

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
# current template upstream:
* https://github.com/lxc/lxc/tree/master/templates
^^All of those can usually be found in /usr/share/lxc/templates. 
* #lxcontainers kanali

### Vagrant-lxc kurulumu (Debian Sid)
sudo vagrant plugin install vagrant-lxc --plugin-version 1.0.0.alpha.2
vagrant plugin install vagrant-lxc
apt-get install linux-headers-$(uname -r|sed 's,[^-]*-[^-]*-,,') virtualbox 
aptitude install  lxc lxctl debootstrap
vagrant init fgrehm/precise64-lxc 
vagrant up --provider=lxc


#### Yapilandirma:
vagrant up --provider=virtualbox

[Kaynak](https://github.com/fgrehm/vagrant-lxc)
