### All about redirection
* Not: tum betiklerin basinda bunun bir bash programi oldugunu belirten
#!/bin/bash satirini sola yaslayarak yaz, bosluk oldugunda hata veriyor.

1 stdout 2 stderror'u temsil eder

* stderr ve stdout 'u dosyaya yazdirmak icin;
```
rm -f $(find / -name core) &> /dev/null 
```

### Functions
* fonksiyonlara arguman atanarak yapacagin istekleri kisaltabilirsin.


[Kaynak:](http://www.linuxdoc.org/HOWTO/Bash-Prog-Intro-HOWTO-3.html)

* Basename: return non-directory portion of a pathname
[Kaynak:](http://pubs.opengroup.org/onlinepubs/007908799/xcu/basename.html)

* How to escape single-quotes within single-quoted strings?

```
alias llz=$'ls -alh |awk \'{print$9}\''
```

[Kaynak:](http://stackoverflow.com/questions/1250079/how-to-escape-single-quotes-within-single-quoted-strings)

* 10'luk tabanda islem yapmak (09 - 1 icin mesela)
```
b=10#$(date +%m)
```

* regex kullanma ornegi;
```
#!/bin/bash
b=10#$(date +%m)
((b=b-1))
c='^test2014'$b'.*02$'
echo "$c"
```

* degisken atamada bosluk birakilmayacak;
```
a="$b"
b=10#$(date +%m)
c=5
```
* sikistirma ve acma icin for donguleri
```
for i in *.csv; do tar jcvf $i.tar.bz2 $i; done
for i in *.tar.gz; do tar -zxvf $i; done
```
* [Kaynak](http://stackoverflow.com/questions/15936003/for-each-dir-create-a-tar-file)

* [Check if a directory exists in a shell script](http://stackoverflow.com/questions/59838/check-if-a-directory-exists-in-a-shell-script)

* `!` Ozel karakterini kullanmak icin `set -o histexpand`

|| ile && arasindaki fark
1.si kosullu sonuc false ise calisiyor,
2.si kosullu sonuc true ise;
Buna gore asagidakiler calismaz;
false && echo howdy!
true || echo howdy!

