####  Deployment  
* yazi content altinda hazirlanir.  
* output altinda SimpleHTTPServer acilarak yayina almadan once yazinin gorunumu  
izlenir.  
* html uyumlu olarak icerik hazirlanir.  
```
pelican content -s publishconf.py  
```
* Deploy edilir.  
```
rsync -avc --delete output/ host.example.com:/var/www/your-site/  
```
* Degisiklik git repo'suna gonderilir.  
#### Not:  
* otomatize eden araclar incelenecek  
* md dosyasinda status: draft ile yazi yayini gizlenir.  
