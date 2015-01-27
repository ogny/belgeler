#### Komutlar


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

* iso'yu flash disk'e yazma (partition'a degil diskin tamamina yazilir.)
```
sudo dd bs=4M if=/path/to/iso of=/dev/sdb oflag=direct && sudo sync
```
* Hata: * Error: Problem adding (is pinentry installed?); giving up
```
eval $(gpg-agent --daemon --pinentry-program /usr/bin/pinentry-curses)
```
* son kolona gore siralama;
```
perl -e "print sort {(split '@', \$a)[-1] <=> (split '@', \$b)[-1]} <>" dosya.adi
```
* ps ciktisini memory-cpu'ya gore siralayip degisiklikleri izleme;
```
watch --differences -n 2  'ps -e -o pid,cmd,pmem,pcpu --sort=-pmem,-pcpu | head -15'
```

* acik kalmis bir oturumu oldurme;
w komut ciktisi ile acik oturumlar bulunur; ornegin;
 16:03:00 up  6:52,  2 users,  load average: 0,18, 0,37, 0,40
 USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
 orkung   tty1                      09:12    6:50m  6.96s  0.00s xinit /home/orkung/.xinitrc -- /etc/X11/xinit/xserverrc :0 -auth
 /tmp/serverauth.ACk3tKw1Kx
 orkung   pts/2    :0               15:41    4.00s  0.00s  0.00s tmux new-session
** ps -dN|grep pts/2 ile orkung'nin tmux oturum PID'i ogrenilir, kill ile oldurulur.

* default web tarayiciyi degistirme;
```
sudo update-alternatives --config x-www-browser 
```
* agdaki tum makina sayisini bul
```
seq 1 255 |parallel -j0 --eta ping -c 1 192.168.1.{} 2>&1 |grep '64 bytes' |wc -l
```
* tum makinalarin mac adreslerini bulma;
```
arp -a -n
```

### Chroot
```
mount -o bind /dev /mnt/mychroot/dev 
mount -t proc none /mnt/mychroot/proc
mount -o bind /sys /mnt/mychroot/sys
mount -o bind /tmp /mnt/mychroot/tmp
chroot /mnt/mychroot /bin/bash 
```
[Kaynak:Gentoo wiki](http://wiki.gentoo.org/wiki/Chroot)

sistem analiz  komutlari
```
iostat -c 1 10
vmstat 1 20
free -m
top -b -n1 | head -5
ps auxf | sort -nr -k 4 | head -5
ps auxf | sort -nr -k 3 | head -5
```
* Unix time'i (epoch) gormek icin;
```
date +%s
```

* Tersi;
```
date -d @EPOCH_TIME +"%d-%m-%Y %T %z
```

* Belli bir pid'de sorun oldugunu dusunursen strace ile pid'de yapilan isin
ciktisina bak;
```
strace -pPID
```

* sistemde default editor neyse onu sudoyla acan komut;
```
sudoedit DOSYA
```

* terminalden bosluk iceren dizin olusturmak icin dizin adini tirnak icinde
  yaziyoruz.

* ps ciktisinda komutun yaptigi isin tamamini processid'den de gorebiliriz;

```
cat /proc/<PID>/cmdline 
```
* belli bir portu aktif olarak dinleyen uygulamalar;

```
netstat -anpt | grep ESTABLISHED | awk '{ print $7 }' | cut -d/ -f2 | sort -u
```

