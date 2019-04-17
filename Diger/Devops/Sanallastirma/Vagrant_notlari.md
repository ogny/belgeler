 platform; 3.13.0-29-generic x86_64 jessie/sid (ubuntu trusty 14.04) 

Virtualbox tabanlı sanallaştırma çözümü, konsol komutlarıyla sanal makina
yönetmeyi sağlıyor.

# kurulumu vagrantup.com'daki paketten yap.

## vagrantfile'dan yapilabilenler

* Coklu makine olusturma 
** makinelere farkli ip'ler atama
** guest'in port'larini host'a yonlendirme (ssh-http vd.)

* yeni bir vagrant box kullanilmak istendiginde asagidaki uyari geliyor.
==> default: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> default: to force provisioning. Provisioners marked to run always will still run.
```
mv .vagrant/machines/default/virtualbox ~/
```
` vagrant init <box_adi>`
` vagrant up`

* Asagidaki hatayi aldiginda 
```
Failed to mount folders in Linux guest. This is usually because
the "vboxsf" file system is not available. Please verify that
the guest additions are properly installed in the guest and
can work properly. The command attempted was:
mount -t vboxsf -o uid=`id -u vagrant`,gid=`getent group vagrant | cut -d: -f3`
vagrant /vagrant
mount -t vboxsf -o uid=`id -u vagrant`,gid=`id -g vagrant` vagrant /vagrant
```
* host sunucuda;
* `virtualbox-guest-additions-iso` paketini kur.
** `vagrant plugin install vagrant-vbguest`
* guest sunucularda; `ln -s /opt/VBoxGuestAdditions-4.3.10/lib/VBoxGuestAdditions
/usr/lib/VBoxGuestAdditions`

#### Kullanımı (eklenecek)
* `vagrant <komut> <sunucu_adi>` # birden çok sunucu gelebilir.
* calıştırırken `--force`  kullan. gereksiz sorulari ignore et.
