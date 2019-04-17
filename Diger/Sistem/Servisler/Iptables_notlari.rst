Iptables Notlari
==================

* vsftpd'nin erisime acilmasi icin; 20 ve 21'e izin ver, ek olarak::
   /etc/sysconfig/iptables-config
   IPTABLES_MODULES="nf_conntrack_ftp"


* Eklenen bir kurali silme (Ornegin accept kuralini)::

    iptables -D INPUT -p  tcp -s IP_Adresi --dport Port_no -j ACCEPT 

* The TCP protocol looks at data as an continuous data stream with a
start and a stop signal. The signal that indicates that a new stream is
waiting to be opened is called a SYN three-way handshake in TCP, and
consists of one packet sent with the SYN bit set. The other end then
either answers with SYN/ACK or SYN/RST to let the client know if the
connection was accepted or denied, respectively. If the client receives
an SYN/ACK packet, it once again replies, this time with an ACK
packet. At this point, the whole connection is established and data can
be sent. During this initial handshake, all of the specific options that will
be used throughout the rest of the TCP connection is also negotiated,
such as ECN, SACK , etcetera

While the datastream is alive, we have further mechanisms to see that
the packets are actually received properly by the other end. This is the
reliability part of TCP. This is done in a simple way, using a Sequence
number in the packet. Every time we send a packet, we give a new
value to the Sequence number , and when the other end receives the
packet, it sends an ACK packet back to the data sender. The ACK
packet acknowledges that the packet was received properly. The
sequence number also sees to it that the packet is inserted into the
data stream in a good order.

Once the connection is closed, this is done by sending a FIN packet
from either end-point. The other end then responds by sending a
FIN/ACK packet. The FIN sending end can then no longer send any
data, but the other end-point can still finish sending data. Once the
second end-point wishes to close the connection totally, it sends a FIN
packet back to the originally closing end-point, and the other end-point
replies with a FIN/ACK packet. Once this whole procedure is done, the
connection is torn down properly.

* iptables was and is specifically built  to work on the headers of the
Internet and the Transport layers

* x portu haric Tum portlari erisime kapatma::

    iptables -A INPUT -p tcp --dport 80 -j ACCEPT
    iptables -A INPUT -p tcp --dport 443 -j ACCEPT

`Kaynak:Superuser <http://superuser.com/questions/769814/how-to-block-all-ports-except-80-443-with-iptables/>`_.

* eth0'a gelen trafik engellensin, istina; eth0:0'a gelen trafik kabul
  edilsin.::

   iptables -A INPUT -i eth0 -d xx:yy:zz:vv -j ACCEPT
   iptables -A INPUT -i eth0 -j DROP

`Kaynak:Superuser <http://www.superuser.com/questions/698081/how-to-block-incoming-traffic-on-a-virtual-interface/>`_.

**Nat table**

- it should only be used to translate the packet's source field or destination field.
- The actual targets that do these kind of things are:

  * DNAT : DMZ'e alma ornek gosterilebilir. Public IP'den gelen paketleri belli bir hedefe yonlendirme.

  * SNAT : 

  * MASQUERADE

  * REDIRECT
   
* Tum aktif kurallari listeleme::

    iptables -L -n -v

* ornek bir `/etc/sysconfig/iptables`::
  # Firewall configuration written by system-config-firewall
  # Manual customization of this file is not recommended.
  *filter
  :INPUT ACCEPT [0:0]
  :FORWARD ACCEPT [0:0]
  :OUTPUT ACCEPT [0:0]
  -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
  -A INPUT -p icmp -j ACCEPT
  -A INPUT -i lo -j ACCEPT
  -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
  -A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT
  -A INPUT -m state --state NEW -m tcp -p tcp --dport 20 -j ACCEPT
  -A INPUT -j REJECT --reject-with icmp-host-prohibited
  -A FORWARD -j REJECT --reject-with icmp-host-prohibited
  COMMIT

iptables -t nat -A PREROUTING -i eth0 -p udp -m udp --dport 514 -j REDIRECT --to-ports 5514


    iptables [-t table] command [match pattern] [action]

iptables -t nat -A POSTROUTING -p tcp --dport 80\
       -j MASQUERADE
