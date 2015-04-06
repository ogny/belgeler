* ozyinelemeli olmayan arama;··
[Kaynak](http://stackoverflow.com/questions/3925337/find-without-recursion)··
-maxdepth parametresi ile··
find DirsRoot/* -maxdepth 0 -type f #This does not show hidden files··
find DirsRoot/ -maxdepth 1 -type f #This does show hidden files··

* icinde mail gecen .php uzantili calistirilabilir dosyalarin tespiti   
```
find . -executable -type f  -name "*.php" |egrep mail
```

* config disindaki dizinlerde jpg dosya var mi arat.
```
find . \( -path -o -path ./.config \) -prune -o -name "*.jpg"  -print
```

* Dosya ve dizin haklarinin web sunucunun erismesi icin ayarlanmasi
```
find * -type d -exec chmod 755 {} \;
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
