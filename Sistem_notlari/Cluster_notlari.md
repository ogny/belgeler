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


