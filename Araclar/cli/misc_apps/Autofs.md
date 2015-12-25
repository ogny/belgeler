```
man autofs
man auto.master
```
* dizini fstab'daki syntax'a gore bagliyor;
  - kullanici olarak baglamasi icin `user` ekle
  - gid,uid belirtirsen baglayamiyor, gerek yok.

* uzun vadede mount edilen cihazlara gore `/etc/autofs/` altinda farkli
  dosyalar olusturmakta fayda var

* hatalari yakalamak icin servisi durdur, root ile `automount -vdf` calistir.

##### Eski
auto.misc ile calistiramadim, auto.template olusturup ona yazdigimda calisiyor,
auto.template'i master'a yazaren basinda `/-`  koydum.
