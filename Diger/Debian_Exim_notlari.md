#### Exim Yapilandirma
###  Disariya e-posta atacak sekilde ayarlama
* Sunucuda exim4 paketinin kurulu olup olmadığı kontrol edilir;
```
dpkg -l | grep exim4   
```
* exim4 satırını görüyorsanız kurulu, görmüyorsanız değildir, kurulu değilse aşağıdaki komutla kurulur;
```
apt-get install exim4
```
* Kurulumda ayarlarını soracak (General type of mail configuration:), ilk soruda  ilk sıradaki "internet site; mail is sent and received directly using SMTP" seçilir, geri kalan tüm default ayarlar korunarak kurulum tamamlanır.
* Exim4 sunucuda zaten kurulu ise ayarları yukarıdaki gibi düzenlemek için;
```
dpkg-reconfigure exim4-config
```
* Kuyrugu gormek;
```
mailq |less
```
* Kuyrugu silmek
```
exim -bp | exiqgrep -i | xargs exim -Mrm
```
