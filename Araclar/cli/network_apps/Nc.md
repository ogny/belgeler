* Belli bir port'a baglanti olusturma
  - Lokal makinada port dinlenir (udp icin -u parametresi ilave etmek yeterli)
```bash
nc -4 -v -l 0.0.0.0 <port>
```
  - uzak makinadan ilgili port'a baglanilir
```bash
nc -v <uzak_sunucu_ip> <port>
```

