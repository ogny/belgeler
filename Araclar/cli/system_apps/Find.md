### Komut ornekleri 

* zamana gore arama kosullari
    - `-mtime` ile belli bir zaman araligi arat.
        +30: 30 gunden daha eski
        -30: 30 gunden daha yeni
        30: tam olarak 30 gun once olusturulmus
    - `-newer` <dosya_adi> belirtilen dosyadan daha yeni

* dosya buyuklugune gore arama kosullari
    - `-size` +<buyukluk>k ile kilobayt cinsinden arat.

* (veya) or operatoru; birden cok farkli aranacak degeri birarada
  sorgulatabilirsin.

```
find . -name '*.csv' -o -name '*.txt' |wc -l
```

* ozyinelemeli olmayan arama;··
[Kaynak](http://stackoverflow.com/questions/3925337/find-without-recursion)··
-maxdepth parametresi ile··
```
find DirsRoot/* -maxdepth 0 -type f #This does not show hidden files··
find DirsRoot/ -maxdepth 1 -type f #This does show hidden files··
```

* icinde mail gecen .php uzantili calistirilabilir dosyalarin tespiti   
```
find . -executable -type f  -name "*.php" |egrep mail
```

* config disindaki dizinlerde jpg dosya var mi arat.
```
find . \( -path -o -path ./.config \) -prune -o -name "*.jpg"  -print
```

* alt dizin exclude etme
```
find . -type d -iname "<aranan_string>" -not -path "*<sub_directory>*"
```

* Dosya ve dizin haklarinin web sunucunun erismesi icin ayarlanmasi
```
find * -type d -exec  chmod 755 {} \;
find * -type f -exec chmod 644 {} \;
```
* dosyalari uzantilarina gore bul ve kopyala;
```
find /share/media/mp3/ -type f -name "*.mp3" -print0 | xargs -0 -r -I file cp \
-v -p file --target-directory=/bakup/iscsi/mp3
```
* dizindeki tum dosyalari ozyinelemeli olmadan bul ve sil
```
find ./* -type f  -maxdepth 0 ! -iname ".*" -exec rm -f {} \;
```
* boyle de bulup sikistiriyor
```
find /home/egem/ptt_quota/ -type f -mtime 10 -name '*.txt' -print | tar jcvf
backup-2015-10-16.tar.bz2 -T -
```
* belli bir araliktaki dosyalari bulma;
```
find <dizin> -newermt "nov 10, 2015" -not -newermt  "nov 11, 2015" \
-exec mv -t ./<tasinacak_dizin>/ {} +
```

* buyukluge gore siralama;
```
find -maxdepth 0 
```

* belli bir tarih araliginda olusan dosyalari bulup tasima;
```
```

* belli bir gruba ait dosyalari bulma, (kullaniciya ait olanlar da bulunabilir mi, arastir,)
```
-group <grup_adi>
```

### Kaynaklar:

* [UsingFind](http://mywiki.wooledge.org/UsingFind)
* [dosya boyutlarina gore arama](http://linuxconfig.org/how-to-use-find-command-to-search-for-files-based-on-file-size)


#### Arastirma
Note that modern versions of find (compatible with POSIX 2008) support + in place of ; and behave roughly the same as xargs without using xargs:

find . -type d -mtime -0 -exec mv -t /path/to/target-dir {} +
This makes find group convenient numbers of file (directory) names into a single invocation of the program. You don't have the level of control over the numbers of arguments passed to mv that xargs provides, but you seldom actually need that anyway. This still hinges on the -t option to GNU mv.
[sto](http://stackoverflow.com/questions/13899746/use-xargs-to-mv-the-find-directory-into-another-directory)


