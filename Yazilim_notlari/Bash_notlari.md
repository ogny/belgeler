### All about redirection
[Kaynak:](http://www.linuxdoc.org/HOWTO/Bash-Prog-Intro-HOWTO-3.html)

1 stdout 2 stderror'u temsil eder

* stderr ve stdout 'u dosyaya yazdirmak icin;
```
rm -f $(find / -name core) &> /dev/null 
```

