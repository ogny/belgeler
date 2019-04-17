* Belli bir port'a baglanti olusturma
  - Lokal makinada port dinlenir (udp icin -u parametresi ilave etmek yeterli)
    icerik gondermek icin de 2. ornek
```bash
nc -4 -v -l 0.0.0.0 <port>
```
  - uzak makinadan ilgili port'a baglanilir
```bash
nc -v <uzak_sunucu_ip> <port>
echo -n 'foo' | nc -u -v <uzak_sunucu_ip> <port>
```

