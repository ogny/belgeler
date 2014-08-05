* Eklenen bir kurali silme (Ornegin accept kuralini)
iptables -D INPUT -p  tcp -s IP_Adresi --dport Port_no -j ACCEPT 

* iptables was and is specifically built  to work on the headers of the
Internet and the Transport layers

