Iptables Notlari
==================


* Eklenen bir kurali silme (Ornegin accept kuralini)

..  code:: sh

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

* x portu haric Tum portlari erisime kapatma 

.. code:: sh

    # Exceptions to default policy
    iptables -A INPUT -p tcp --dport 80 -j ACCEPT
    iptables -A INPUT -p tcp --dport 443 -j ACCEPT

`Kaynak:Superuser <http://superuser.com/questions/769814/how-to-block-all-ports-except-80-443-with-iptables/>`_.

* eth0'a gelen trafik engellensin, istina; eth0:0'a gelen trafik kabul edilsin.

.. code:: sh

   iptables -A INPUT -i eth0 -d xx:yy:zz:vv -j ACCEPT
   iptables -A INPUT -i eth0 -j DROP

`Kaynak:Superuser <http://www.superuser.com/questions/698081/how-to-block-incoming-traffic-on-a-virtual-interface/>`_.


**Nat table**

* it should only be used to translate the packet's source field or destination field.
* The actual targets that do these kind of things are:
  ** DNAT : DMZ'e alma ornek gosterilebilir. Public IP'den gelen paketleri
  belli bir hedefe yonlendirme.
  ** SNAT : 
  ** MASQUERADE
  ** REDIRECT
   

