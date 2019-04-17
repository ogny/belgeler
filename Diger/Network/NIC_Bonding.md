Tanim: combining both of the computer's interfaces into a single interface

OS can alternate which interface it uses to send traffic, or it can gracefully
fail over between them in the event of a problem. You can even use it to
balance your traffic between multiple wide area network (WAN) connections, 

bonding two WAN links gives you load balancing and fault tolerance between
them, but it does not double your upstream throughput, since each connection
(such as a Web page HTTP request) has to take one or the other route.

Uygulama
---

bond physical interfaces em1 and em2 with a bond device labeled bond0. If
NetworkManager is installed, ensure it is disabled.  

* Check the status of NetworkManager:
```
chkconfig –-list | grep NetworkManager
NetworkManager 0:off 1:off 2:off 3:off 4:off 5:off 6:off
```
* Disable NetworkManager:
```
service NetworkManager stop
chkconfig NetworkManager off
```
* As the root user, execute the following command which creates a file named
bonding.conf within the /etc/modprobe.d/ directory needed to create a bonded
device for multiple network interfaces. 
```
echo "alias bond0 bonding" > /etc/modprobe.d/bonding.conf
```
* As the root user, create a backup of the ifcfg-em1 & ifcfg-em2 files, create
the ifcfg-bond0 file and edit the ifcfg-em1 & ifcfg-em2 configuration files
found within /etc/sysconfig/networkscripts.
An example can be seen below.
```
cp /etc/sysconfig/network-scripts/ifcfg-em1 /etc/sysconfig/networkscripts/ifcfg-em1.bkup
cp /etc/sysconfig/network-scripts/ifcfg-em2 /etc/sysconfig/networkscripts/ifcfg-em2.bkup
DEVICE=bond0
IPADDR=172.25.6.64
NETMASK=255.255.255.0
ONBOOT=yes
BOOTPROTO=none
USERCTL=no
BONDING_OPTS="mode=1 miimon=100"
NM_CONTROLLED="no"

cat /etc/sysconfig/network-scripts/ifcfg-bond0
DEVICE="bond0"
BONDING_OPTS="mode=1 miimon=100 primary=em1"
NM_CONTROLLED="no"
IPADDR="10.16.142.51"
NETMASK="255.255.248.0"
GATEWAY="10.16.143.254"
ONBOOT="yes"

# cat /etc/sysconfig/network-scripts/ifcfg-em1
DEVICE="em1"
BOOTPROTO="none"
HWADDR="00:25:B3:A8:6F:18"
IPV6INIT="no"
NM_CONTROLLED="no"
ONBOOT="yes"
TYPE="Ethernet"
UUID="3db45d28-e63c-401b-906a-ef095de4fc1e"
SLAVE="yes"
MASTER="bond0"

# cat /etc/sysconfig/network-scripts/ifcfg-em2
DEVICE="em2"
BOOTPROTO="none"
HWADDR="00:25:B3:A8:6F:19"
IPV6INIT="no”
NM_CONTROLLED="no"
ONBOOT="yes"
TYPE="Ethernet"
UUID="7d29d87f-52bb-4dc6-88ca-d0857c7d7fd9"
SLAVE="yes"
MASTER="bond0"

# cat /etc/sysconfig/network-scripts/ifcfg-bond0
ifcfg-bond0
DEVICE=bond0
IPADDR=<IP_Adresi>
NETMASK=255.255.255.0
ONBOOT=yes
BOOTPROTO=none
USERCTL=no
BONDING_OPTS="mode=1 miimon=100"
```
echo "alias bond0 bonding" > /etc/modprobe.d/bonding.conf
```

* After all the network scripts are configured, restart the network service via
the command:
```
service network restart
Shutting down interface bond0: [ OK ]
Shutting down loopback interface: [ OK ]
Bringing up loopback interface: [ OK ]
Bringing up interface bond0: [ OK ]
```

diger adlari;

1. Link aggregation
2. Channel Bonding
3. NIC Bonding
4. NIC teaming
5. Network card Bonding
6. Ethernet bonding
7. Trunking
8. Etherchannel
9. Multi-link truning(MLT)
10.Network bonding
11.Network Fault Tolerance(NFT)
12.Port channel
13.Smartgroup
14.EtherTrunk

Kaynaklar
---
* [NIC Bonding In Linux](http://www.linuxnix.com/2010/11/nic-bonding-in-linux.html)
* [What can you do with a second Ethernet port?](http://archive09.linux.com/feature/133849)
* [2014 - Deploying Oracle Database 12c on Red Hat Enterprise Linux
  6](https://access.redhat.com/sites/default/files/attachments/deploying-oracle-12c-on-rhel6_1.2_1.pdf)




/etc/modprobe.d/bonding.conf
cat /proc/net/bonding/bond0
Ethernet Channel Bonding Driver: v3.6.0 (September 26, 2009)

Bonding Mode: fault-tolerance (active-backup)
Primary Slave: None
Currently Active Slave: eth0
MII Status: up
MII Polling Interval (ms): 100
Up Delay (ms): 0
Down Delay (ms): 0

Slave Interface: eth0
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: e4:1f:13:78:f7:04
Slave queue ID: 0

Slave Interface: eth1
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: e4:1f:13:78:f7:06
Slave queue ID: 0

