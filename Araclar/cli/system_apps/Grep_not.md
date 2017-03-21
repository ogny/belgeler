* dizinde arama
```
egrep -wir $1 --exclude-dir=<dizin> *
```

* multiple string arama;
```
 netstat -tanpl |egrep '(5432|5433)'
```
