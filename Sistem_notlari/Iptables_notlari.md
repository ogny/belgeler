* Eklenen bir kurali silme (Ornegin accept kuralini)
iptables -D INPUT -p  tcp -s IP_Adresi --dport Port_no -j ACCEPT 


