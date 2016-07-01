### Crm Resource'lari ile cluster yapılandırması
* crm help Overview
* crm help topics

  primitive apache apache \
    params configfile=/disk0/etc/apache2/site0.conf

crm ls
cibstatus        help             site             
cd               cluster          quit             
end              script           exit             
ra               maintenance      bye              
?                ls               node             
configure        back             report           
cib              resource         up               
status           corosync         options          
history          

cib nedir?
cib/           CIB shadow management



Usage: crm_resource (query|command) [options]
Options:
 -?, --help     This text
 -$, --version      Version information
 -V, --verbose      Increase debug output
 -Q, --quiet      Print only the value on stdout

crm_resource --list-standards
ocf
lsb
service
stonith

crm_resource --list-agents=lsb
crm_resource --list-agents=ofc
crm_resource --list-agents=service
crm_resource --list-ocf-providers

* service ve lsb agent'lari ayni.



Heartbleed ve pacemaker kullanilarak yapilabiliyor
Pacemaker 
[Kaynak](http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html-single/Clusters_from_Scratch/index.html)

Pacemaker is a cluster resource manager.
It achieves maximum availability for your cluster services (aka. resources) by
detecting and recovering from node and resource-level failures by making use of
the messaging and membership capabilities provided by your preferred cluster
infrastructure (either Corosync or Heartbeat).

### Heartbeat as a Cluster Messaging Layer
[Kaynak:](http://www.linux-ha.org/doc/users-guide/users-guide.html)
* Kurulum;
apt-get install heartbeat cluster-glue cluster-agents pacemaker

Heartbeat supports cluster communications over the following network link
types:
unicast UDP over IPv4;
broadcast UDP over IPv4;
multicast UDP over IPv4;

The heartbeat layer has an API which provides the following classes of
services:

intra-cluster communication - sending and receiving packets to cluster nodes
configuration queries
connectivity information (who can the current node hear packets from) - both
for queries and state change notifications
basic group membership services


