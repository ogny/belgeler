* session timeout suresinin uzatmak;
php.ini'de session.gc_maxlifetime degeri arttirilir.

* kullanici girislerinde sorun olusuyorsa session'larin tutulacagi dizinin yazma izinlerini kontrol et;
genelde session'lar gecici dosyalar oldugu icin /tmp altinda tutuluyor, /tmp'nin izinlerini 777 yaptiginda duzelebiliyor. 

* Php5.5'den itibaren apc default gelmiyor. Onun yerine zend opcache var, php5
ile birlikte kuruluyor.

* xdebug aktif etme;
```
aptitude install php5-xdebug
```
** /etc/php5/fpm/php.ini'ye eklenecek satilar;
```
;zend_extension = /usr/lib/php5/20090626/xdebug.so
;xdebug.remote_enable=1
;xdebug.remote_host=localhost
;xdebug.remote_port=9000
xdebug.profiler_enable_trigger = 1
xdebug.profiler_output_dir = /tmp
```
