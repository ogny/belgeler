=============================================
Centos'la calisirken karsilasilan durumlar
=============================================

Kurulum
=======

Kurulum medyası: CentOS 6.6 x86_64 minimal
`Indirme adresi <http://ftp.linux.org.tr/centos/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>`_
işletim sistemi text mode'da kurulur. Grub ekranında tab'a
basıp editleyin. en sona text yazıp enter'a basın.


:date: 2015-01-19

#. chef'le paket kurdugumda asagidaki uyariyi verdi, google'la::

        restore selinux security context

#. default gelmeyen paketler::

        yum install -y vim-enhanced screen man man-pages

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

#. ulimit'leri ayarlamak icin limits.conf disinda asagidakinde de max number of
   process 'i duzenle::

    /etc/security/limits.d/90-nproc.conf 

#. Centos6'da servisleri listelemek::

   chkconfig --list 

Kullanici yonetimi
------------------

#. Encrypt edilmis parola olusturulur::

    openssl passwd -crypt  <parola>

#. Kullaniciya bu parola tanimlanir::

    useradd  -p <encrypted_parola> <kullanici_adi>

#. Man kurulumu

#. Timezone'u degistirme::

    mv /etc/localtime /etc/localtime.bak
    ln -s /usr/share/zoneinfo/Europe/Istanbul /etc/localtime

#. kullanciyi sudo ile yetkilendirme

[kullanicinin sudo 
yetkilendirmesi](http://www.cyberciti.biz/faq/linux-sudo-allows-people-in-group-admin/)

#. export HOSTNAME= calismiyor, bunun yerine 
```
HOSTNAME=<new_hostname>
export HOSTNAME
```

#. initd'ye yeni servis ekleme (supervisor);
```

cat << EOF > /etc/init.d/supervisord
#!/bin/bash
#
# supervisord   This scripts turns supervisord on
#
# Author:       Mike McGrath <mmcgrath@redhat.com> (based off yumupdatesd)
#
# chkconfig:    - 95 04
#
# description:  supervisor is a process control utility.  It has a web based
#               xmlrpc interface as well as a few other nifty features.
# processname:  supervisord
# config: /etc/supervisord.conf
# pidfile: /var/run/supervisord.pid
#
# source function library
. /etc/rc.d/init.d/functions
RETVAL=0
start() {
    echo -n $"Starting supervisord: "
    daemon supervisord
    RETVAL=$?
    echo
    [ $RETVAL -eq 0 ] && touch /var/lock/subsys/supervisord
}
stop() {
    echo -n $"Stopping supervisord: "
    killproc supervisord
    echo
    [ $RETVAL -eq 0 ] && rm -f /var/lock/subsys/supervisord
}
restart() {
    stop
    start
}
case "$1" in
  start)
    start
    ;;
  stop)
    stop
    ;;
  restart|force-reload|reload)
    restart
    ;;
  condrestart)
    [ -f /var/lock/subsys/supervisord ] && restart
    ;;
  status)
    status supervisord
    RETVAL=$?
    ;;
  *)
    echo $"Usage: $0 {start|stop|status|restart|reload|force-reload|condrestart}"
    exit 1
esac
exit $RETVAL
EOF

chkconfig --add supervisord
chkconfig supervisord on
/etc/init.d/supervisord start
supervisorctl start all
```
