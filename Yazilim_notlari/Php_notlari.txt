* session timeout suresinin uzatmak;
php.ini'de session.gc_maxlifetime degeri arttirilir.


* kullanici girislerinde sorun olusuyorsa session'larin tutulacagi dizinin yazma izinlerini kontrol et;
genelde session'lar gecici dosyalar oldugu icin /tmp altinda tutuluyor, /tmp'nin izinlerini 777 yaptiginda duzelebiliyor. 
