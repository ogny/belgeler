* yuklu sertifikalari gorme
```
openssl s_client -connect localhost:443 -showcerts |less
```

* sertifika suresinin ne zaman dolacak
```
openssl x509 -enddate -in /etc/apache2/ssl/*.crt -noout
```

* sertifikanin acilimi
```
openssl x509 -noout -text -in www.tasit.com.crt
```

* sertifikanin heartbleed/poddle acigi olup olmadigini gorme
```
openssl s_client -connect <ip>:443 -ssl3 
```

