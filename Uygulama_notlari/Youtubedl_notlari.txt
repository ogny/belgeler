repo'dan degil github'taki yoldan indir;
```
sudo curl https://yt-dl.org/latest/youtube-dl -o /usr/local/bin/youtube-dl  
sudo chmod a+x /usr/local/bin/youtube-dl  
```
* vidyoyu indirirken;
    ** hatalari atlama; -i
    ** dosya adindaki karakterleri ASCII ile sinirlandirma; --restrict-filenames
    ** vidyonun kucuk resmini olusturma; --write-thumbnail
    ** dizin adi/dosya adi belirtme;  -o 'dosyanin/yolu/%(title)s.%(ext)s'
Ornek indirme komutu;
```
youtube-dl -i --write-thumbnail -o \
'~/Copy/multimedia/video/yoga/%(title)s.%(ext)s'  --restrict-filenames \
zHf1KpmlhQU
```



