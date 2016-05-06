### Active/Passive Postgresql Cluster, Shared Storage, via Pacemaker and Corosync

#### Yapilanlar

* kurulan paketler
```
yum install -y pacemaker corosync fence-agents lsscsi crmsh
```
* Servisler acilista baslasin
```
chkconfig corosync on
chkconfig pacemaker on
```

service {
# Load the Pacemaker Cluster Resource Manager
name: pacemaker
ver: 1
}

* corosync.conf'u olusturup ilk satirlara asagidaki bolumu ekle.
  memberaddr'lari ve bindnetaddr'i duzenle
```
cp /etc/corosync/corosync.conf.example.udpu /etc/corosync/corosync.conf
aisexec {
# Run as root - this is necessary to be able to manage resources with
Pacemaker
user: root
group: root
}
```



#### Ek Bilgi
```
corosync-1.4.7-2.el6.x86_64 
fence-agents-4.0.15-8.el6_7.2.x86_64 
pacemaker.x86_64 0:1.1.12-8.el6_7.2 
rpm -qa |grep 'libqb\|glibc'
glibc-common-2.12-1.166.el6.x86_64
glibc-2.12-1.166.el6.x86_64
libqb-0.17.1-1.el6.x86_64
```
* pacemaker dizini; /var/lib/pacemaker/cib/

##### pacemaker Internal Components

* CIB (aka. Cluster Information Base)
* CRMd (aka. Cluster Resource Management daemon)
* PEngine (aka. PE or Policy Engine)
* STONITHd

_"Never edit the cib.xml file manually. Ever. I’m not making this up."_


The cluster is written using XML notation and divided into two main sections:
configuration and status.
The configuration section itself is divided into four parts:
Configuration options (called crm_config)
Nodes
Resources
Resource relationships (called constraints)

wget -P /etc/yum.repos.d/ http://download.opensuse.org/repositories/network:/ha-clustering:/Stable/CentOS_CentOS-6/network:ha-clustering:Stable.repo


/data/graphiteuser/graphite-web/.venv/bin/python2.7 /data/graphiteuser/graphite-web/.venv/bin/gunicorn -c /data/graphiteuser/graphite-web/graphite_gconfig.py graphite.wsgi:application

Resource silme;
crm resource stop <Resource>
crm configure delete <Resource>

grup silme
 crm configure delete group PGCLUSTER
crm configure primitive PGSQL ocf:heartbeat:pgsql params pgctl="/usr/pgsql-9.5/bin/pg_ctl" pgdata="/var/lib/pgsql/9.5/data" op monitor interval="30" timeout="30" depth="0"


Onemli: crm resource cleanup PGSQL

NODE2 ~]$ sudo crm resource failcount PGSQL show node2.example.net
scope=status name=fail-count-PGSQL value=0
Now back to some real testing, lets bring node1 back online in the pool and see what happens. N

crm configure order PGCLUSTER_START_ORDER inf: VIP DBFS PGSQL


