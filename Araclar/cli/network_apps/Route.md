* Erisim engelleme
```
ip route add blackhole <IP>/32
ip route delete IP/32
```
* Gateway ekleme
```
/sbin/route add -net 0.0.0.0 gw <IP> eth0
```
