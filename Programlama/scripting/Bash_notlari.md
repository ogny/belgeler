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

