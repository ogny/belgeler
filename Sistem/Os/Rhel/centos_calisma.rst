=============================================
Centos'la calisirken karsilasilan durumlar
=============================================

:date: 2015-01-19

#. chef'le paket kurdugumda asagidaki uyariyi verdi, google'la::

        restore selinux security context

#. 7.0'da vim ve screen default gelmiyor::

        yum install -y vim-enhanced screen

#. Centos'a chef-server icin 2 paket var; chef-server ve chef-server-core
   Bizim chef-server'da sadece core kurulu, 1. paketin ne ise yaradigini
   anlayamadim. Ayrica surumu de core'a gore eski::

   chef-server 11.1.6-1.el6
   chef-server-core 12.0.1-1

#. yum update/upgrade farki: Upgrade will delete obsolete packages, while
   update will preserve them.


#. sunucunun fiziksel mi sanal mi oldugunun kontrolu::

        lscpu |grep Hypervisor

# Calisan servis isimleri::

        netstat -anpt | grep ESTABLISHED | awk '{ print $7 }' | cut -d/ -f2 | sort -u

#. Beware of Red Hat's Security Enhance Linux (SELinux). By default on RHEL 6.5
   64-bit I had to use restorecon so sshd can read $HOME/.ssh.. without this,
   ssh from a client kept prompting for a password even though I setup
   everything for password-less ssh.::

        restorecon -Rv ~/.ssh

#. Yum repo'daki paket versiyonunu goruntuleme;

#. ulimit'leri ayarlamak icin limits.conf disinda asagidakinde de max number of
   process 'i duzenle::

    /etc/security/limits.d/90-nproc.conf 

#. Centos6'da servisleri listelemek::

   chkconfig --list 

Kullanici yonetimi
------------------

* Encrypt edilmis parola olusturulur::

    openssl passwd -crypt  <parola>

* Kullaniciya bu parola tanimlanir::

    useradd  -p <encrypted_parola> <kullanici_adi>





