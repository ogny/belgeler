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
update-alternatives --set {editor, x-www-browser, etc.}
```

* Bütün mevcut ethernet modüllerini listelemek için;
```
modprobe -l -t drivers/net -a \*
```

### Kullanici haklari;
* dizinde herkese yazma hakki verildiyse herhangi bir kullanici o dizindeki bir
dosyayi silebilir.

* buyuk dosyayi bolme, birlestirme;
```
split -b 10MB dosya_adi 
cat dosya_adi* > birlestirilmis_dosya_adi
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

* terminal'de epoch timestamp'i convert etme;
```
 date -d @<timestamp>
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

* Default printer'i bulma;

```
lpstat -p -d
```

* Default printer'a is gonderme (cups-bsd paketinin lpr binary'sini kullan)

| What to Type              | What it Does                                                                    |
| ---                       | ---                                                                             |
| `lpr -Pmmlab file1`       | send "file1" to the printer                                                     |
| `lpstat -a`               | list all the printers                                                           |
| `lpq -Pprinter_name`      | see who is also printing to the printer and list the print job numbers          | 
| `lprm -Pprinter_name 200` | remove print job #200 from a printer. You can only do that if you own that job  | 

```
convert 2015-02-06-152435_1366x768_scrot.png -quality 100 -alpha off -density \
600 -set units PixelsPerInch 1366x768px-noalpha.pdf
```

* Centos locale tanimlama
```
localedef -v -c -i en_US -f UTF-8 en_US.UTF-8
```
* belli bir aga ulasimi engelleme
```
ip route add blackhole <ip/subnet>
```

* load average:(uptime ciktisi)
```
```
load average is the run-queue utilization averaged over the last minute, the
last 5 minutes and the last 15 minutes. The run-queue is a list of processes
waiting for a resource to become available inside the Linux operating system. 



* sadece dizinleri sirala

```
tree -dfi -L 1 "$(pwd)"
```

* kullaniciya ait tum process'leri oldurme
```
pkill -9 -u `id -u username`
```
* belli bir dizini iceren process'leri oldurme;
```
ps -efd |grep -e ' <dizin>' |grep -v grep | awk '{print $2}' | xargs kill -9
```

* aktif baglantilari yapan uygulamalar
```
netstat -anpt | grep ESTABLISHED | awk '{ print $7 }' | cut -d/ -f2 | sort -u
```

* iso'yu cd'ye vey flash disk'e yazma (partition'a degil diskin tamamina yazilir.)
```
sudo wodim -eject -tao speed=4 dev=/dev/sr<number> ~/path_to.iso
sudo dd bs=4M if=/path/to/iso of=/dev/sdb oflag=direct && sudo sync
```

* dosyanin modification date'ini duzenleme;
```
touch -t yilaygunsaat oldfile
```
* ekran karti ozellikleri
```
lspci -knn|grep -iA2 vga
```

* kullaniciyi spesifik bir ev dizininde olusturma; (hali hazirdaki bir gruba eklemek icin -g <grup_adi>>) 
```
useradd -d /home/james jim
```
* yetkisiz kullanici olustur:
```
groupadd test && useradd -g test test
```

* kullaniciyi gruba ekleme
```
sudo gpasswd -a <kullanici> <grup>
```

* dmesg log'unda tarih gorme; (centos6.6)
```
vi /etc/rsyslog.conf
`kern.* /var/log/kern.log
/etc/init.d/rsyslog restart
tailf /var/log/kern.log
```
 
* base64 ile (de/en)code etme;
```
echo -n 'test' | base64
echo -n dGVzdA== | base64 -d
```

* sadece link'leri listele;
```
ls -l `find ./ -maxdepth 1 -type l -print`
```

* bir process ne kadar suredir calisiyor?
```
ps -p "$$" -o etime=
```

* fsck yapmamasi icin;
```
This filesystem will be automatically checked every 27 mounts or
180 days, whichever comes first.  Use tune2fs -c or -i to override.
```

* tanimli bir ip'yi elle silme
```
ip a add 10.50.100.5/24 dev eth0
```

```
echo -n '<parola>'  | md5sum | cut -d' ' -f1 
SELECT md5('<parola>');
```

#### Kaynaklar
* [The lpr command](https://cmgm.stanford.edu/classes/unix/lpr.html)


* Bozuk Karakter sorunu (karakter seti cevirme)
```
iconv -f old-encoding -t new-encoding file.txt > newfile.txt
```

```
google-chrome --app="https://open.spotify.com/collection/tracks"
A=$(ls -t ~/Downloads/*.ovpn | head -n1)
tree -dfi -L 1  |less
sudo sshuttle --remote user@ip 0/0 --exclude ip --listen 0.0.0.0
desktop -g 1920x1080 -u orkung 192.168.56.2
VBoxHeadless --startvm mswin7 
gotty -w vim ~/Raspberry_Pi_Webpage_Display_Script.sh 
git fetch --all && git pull origin develop
git diff --color | diff-so-fancy 
w | cut -d " " -f 1 -
make html && make serve
make github 
git add -A && git commit -a -m "san disk yazisi eklendi" && git push --all
curl -XDELETE http://<ip>:4567/clients/<client_name>
fd -e sh --exec wc -l
youtube-dl --extract-audio --audio-format mp3 cqOaTgBC3-0
printenv
ranger --copy-config=all 
vim -S template.vim  
ansible-playbook -i tests/inventory tests/test.yml 
ansible-playbook -i inventory/office_dsmart cm/office_dsmart.yml -vvv
BoxManage modifyvm mswin7 --bridgeadapter3 enp0s25 
VBoxManage showvminfo mswin7 |grep NIC
mtr --report <IP>
ssh -i "/home/orkung/.ssh/orkung_aof.pem"
ubuntu@ec2-35-178-124-227.eu-west-2.compute.amazonaws.com
rdesktop -u orkung 192.168.56.2 -g 1920x1080
icdiff
```
* satirlari sutuna cevirme
```
tr '\n'''
```
