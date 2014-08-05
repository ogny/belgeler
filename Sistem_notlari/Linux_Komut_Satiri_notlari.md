#### Komutlar

* icinde mail gecen .php uzantili calistirilabilir dosyalarin tespiti   
```
find . -executable -type f  -name "*.php" |egrep mail
```

* config disindaki dizinlerde jpg dosya var mi arat.
```
find . \( -path -o -path ./.config \) -prune -o -name "*.jpg"  -print
```

* Dosya ve dizin haklarinin web sunucunun erismesi icin ayarlanmasi
```
find * -type d -exec chmod 755 {} \;
find * -type f -exec chmod 644 {} \;
```

* Komuttan sonra gelecek ozel karakterleri yorumlama. Ornegin; -var- diye bir dizin var ve silmek istiyoruz.
```
rm -rf -- -var-
```

* Deb paketini kurmadan once icine bakma
```
dpkg-deb --contents
```

* Sistem geneli hakkinda bilgi sahibi olmak icin;
```
lsmod : Yüklenmiş modulları gösterir
df -h : Takili harddiskteki bos yerleri ve kullanimda olan alanlarin oranini gösterir.
hdparm -i /dev/hda  : HDA harddiskinizin bütün özelliklerini gösterir.
uname -a : Kullanılan kernelin bütün özelliklerini gösterir.
cat /proc/cpuinfo : CPU'nuzun özelliklerini gösterir.
cat /proc/1/comm : baslaticini gosterir.
```

* Convert a file from ISO-8859-1 (or whatever) to UTF-8 (or whatever)
```
tcs -f 8859-1 -t utf /some/file
```

* bir dosyadaki dns sunucudak A kayitlarini  duzgun sekilde ekrana yazdirmak icin
```
cat /tmp/domains.txt | xargs -iX dig +nocmd X A  +multiline +noall +answer
```

* Dizin icerisindeki tum md uzantili dosyalari txt yapar.
```
rename 's/\.md$/\.txt/' *.md
```

* tcp portundan calisan bir uygulamayi oldurme; (ornek 80)
```
fuser -k -n tcp 80  
```

* Ayni dosyada string ifadeyi degistirmek icin;
```
perl -pi -e 's/OldText/NewText/g' 
```

* awk'da bir karakter belirtip tekrar degisken verdigimde, karakterden sonra degiskenin degerini yazdiriyor, ornek:
```
ls -alh content |awk '{print$9}' |
trt1.smil
ls -alh content |awk '{print$9}' | cut -d "." -f 1
trt1
ls -alh |  awk '{ print $9 "." $9 }'
trt1.smil.trt1.smil
```

* for donugusuyle belirli bir dosya araligini yakalayip silme.
```
for i in {471..1118}; do egrep -l 'info' $i* 2>/dev/null | xargs rm; done
```

* yukaridaki for dongusunu 0-22 n+1 olarak adlandirilmis dizinler icin  recursive calistirma
```
for i in {0..22}; do for j in {471..1118}; do egrep -l 'info' $i/$j* 2> /dev/null;  xargs rm; done; done;
```

* inotify-tools
```
IN_ACCESS ve IN_REMOVE'u izlememiz lazim. 2.si var mi?
```

* pwgen ile parola olusturma
```
pwgen -syB
```

* Shred: Disk uzerine random veri basar. verilerin geri okunmasini engeller
```
apt-get install coreutils
shred -vfz -n 10 /dev/sda5
```

* Ontanimli ayarlari degistirmek icin;
```
update-alternatives --config {editor, x-www-browser, etc.}
```

* Bütün mevcut ethernet modüllerini listelemek için;
```
modprobe -l -t drivers/net -a \*
```

### Kullanici haklari;
* dizinde herkese yazma hakki verildiyse herhangi bir kullanici o dizindeki bir
dosyayi silebilir.

* buyuk dosyayi bolme;
split -b 10MB dosya_adi 
