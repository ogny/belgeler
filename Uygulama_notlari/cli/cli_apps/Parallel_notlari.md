* For each line of input GNU parallel will execute command with the line as
arguments. If no command is given, the line of input is executed. Several
lines will be run in parallel. GNU parallel can often be used as a substitute
for xargs or cat | bash.

* Dosya sikistirma,acma;
```
parallel --gnu -j-2 --eta gzip ::: *.uzanti
parallel --gnu -j-2 --eta gunzip ::: *.uzanti.gz
```
[Kaynak:](http://codextechnicanum.blogspot.com.tr/2013/07/compression-of-files-in-parallel-using.html)
