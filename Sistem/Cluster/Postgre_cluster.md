### Active/Passive Postgresql Cluster, Shared Storage, via Pacemaker and Corosync

#### Yapilanlar

* kurulan paketler
```
yum install -y pacemaker corosync fence-agents lsscsi 
wget -P /etc/yum.repos.d/ http://download.opensuse.org/repositories/network:/ha-clustering:/Stable/CentOS_CentOS-6/network:ha-clustering:Stable.repo
yum install -y crmsh
```
* Servisler acilista baslasin
```
chkconfig corosync on
chkconfig pacemaker on
vi /etc/corosync/service.d/pcmk
service {
# Load the Pacemaker Cluster Resource Manager
name: pacemaker
ver: 1
}
```

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

Resource silme;
crm resource stop <Resource>
crm configure dsupervisorctl  -c supervisord_conf/supervisord.inielete <Resource>

grup silme
 crm configure delete group PGCLUSTER
crm configure primitive PGSQL ocf:heartbeat:pgsql params pgctl="/usr/pgsql-9.5/bin/pg_ctl" pgdata="/var/lib/pgsql/9.5/data" op monitor interval="30" timeout="30" depth="0"


Note, the the crm resource cleanup command will attempt to (re)start the resource as well.
crm resource cleanup PGSQL

NODE2 ~]$ sudo crm resource failcount PGSQL show node2.example.net
scope=status name=fail-count-PGSQL value=0
Now back to some real testing, lets bring node1 back online in the pool and see what happens. N

crm configure order PGCLUSTER_START_ORDER inf: VIP DBFS PGSQL


[root@ipam-fatih-report-1 sensu]# crm configure primitive HA-sensu lsb:sensu-client op monitor interval=5s
ERROR: error: unpack_resources: Resource start-up disabled since no STONITH resources have been defined
   error: unpack_resources:     Either configure some or disable STONITH with the stonith-enabled option
   error: unpack_resources:     NOTE: Clusters with shared data need STONITH to ensure data integrity
Errors found during check: config not valid
Do you still want to commit (y/n)? y
Call cib_apply_diff failed (-62): Timer expired
ERROR: could not replace cib (rc=62)
INFO: offending xml: <diff format="2">
  <change operation="create" path="/cib/configuration/resources" position="0">
    <primitive id="HA-sensu" class="lsb" type="sensu-client">
      <operations>
        <op name="monitor" interval="5s" id="HA-sensu-monitor-5s"/>
      </operations>
    </primitive>
  </change>
</diff>

[root@ipam-fatih-report-1 sensu]# crm configure primitive HA-sensu lsb:sensu-server op monitor interval=5s
ERROR: Cannot create HA-sensu:primitive: Found existing HA-sensu:primitive

primitive DBFS Filesystem \
        params device="/dev/mapper/mpathep1" directory="/dbvolume" fstype=ext4 \
        op monitor interval=10 timeout=40 depth=10
