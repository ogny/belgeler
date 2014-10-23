* nginx'in site icerigini cache'leyip cache'leyemedigini gormek icin;
curl --head domain.tld
ciktisindaki X-Cached degerlerine bakiyoruz, ornek degerler;
UPDATING - HIT - EXPIRED

## HttpProxyModule
* __proxy_read_timeout__: default'u 60sn. o sure boyunca backend sunucudan veri okumaya calisiyor, yanit alamazsa hata dondur.

* To get around the open file limit, do the following (for Ubuntu):

/etc/sysctl.conf
Add the following line:
fs.file-max = 65000

/etc/security/limits.conf
Add the following lines:
www-data       soft    nofile   9000
www-data       hard    nofile   65000

(restart for the above to take affect)

/etc/pam.d/common-session
Add the following line:
session required pam_limits.so

sudo sysctl -p

/etc/nginx/nginx.conf
Add the following line:
worker_rlimit_nofile 9000;

* HttpProxyModule 
proxy_buffer directive'leri onemli degil. arkadaki sunucudan kucuk bir response almaya bakiyor, 8k | 16k kullaniliyor.


* VOD Notlari
VOD işlemi için dosyanın stream edilmesi gerekir, ancak nginx ile dosyayı stream etmediğimiz halde stream ediyormuş gibi bir kullanıcı deneyimi sunabiliyoruz
pseudo-streaming denilen bu metodda sunucuya dosyanın şu saniyesi veya şu baytından itibaren gönder diyoruz.
MP4 ve FLV için ayrı ayrı modüller gerekiyor (mod_mp4, mod_flv)
temel olarak iki format arasındaki fark
dosya_adi.mp4?start=5 (5 burada videonun 5. saniyesini ifade ediyor. ondalıklı da olabilir, 5,2 vb)
dosya_adi.flv?start=102225 (102225 videonun bayt cinsinden nerede olduğu)
CDN e göre dosya adından sonra gönderilen parametre değişebilir Nginx için "start"
mp4 kullanmaya karar verdik mp4 için config;
/etc/nginx/sites-enabled/default içine satırları eklenmeli (modül versiyonu eskiyse son iki satır hata verecektir)

/vod/ kök dizine göre relativ yol

location /vod/ {
    mp4;
    mp4_buffer_size       1m;
    mp4_max_buffer_size   5m;
//    mp4_limit_rate        on;
//    mp4_limit_rate_after  30s;
}

bu klasöre atılacak dosyalar, h264 görüntü, aac ses içeren mp4 dosyaları olmalı
bu formatta olmayan dosyalar dönüştürülmeli (temel komut optimize edilebilir)
ffmpeg -i gelen_dosya.uzanti -vcodec libx264 -acodec libfaac -movflags faststart olusandosya.mp4
eğer ffmpeg versiyonu eskiyse -movflags faststart çalışmayacak
qt-faststart olusandosya.mp4 olusandosya-2.mp4 şeklinde tekrar işleme tabii tutulması gerekecek.

"-movflags faststar" parametresi ve qt-faststart ile yapılmaya çalışılan işlem "moov atom" denen dosya ile ilgili bilgileri içeren kısmın dosyanın başlangıcına taşınması.


* proxy_redirect
http://wiki.nginx.org/HttpProxyModule
* Nginx Caching'e mudahale etme; expires / Cache-Control

* nginx caching alaninda icerik olusmuyorsa;
cache'te tutulacak ayarlarda su ekleme yapilir;
add_header              Cache-Control "must-revalidate, proxy-revalidate";



# Backend windows ise upstream.conf'ta belirtilen;
keepalive degerini 32'ye indirelim, daha fazla ayni anda
gelen istegi karsilayamaz IIS.

# Notasyon; sadece belirtilen dizini al demek
location = /
sadece ana sayfayi al.

# trtsecim.net'te caching'i ayarlarken post ve get icin 2 ayri config olusturduk.
Sorgulardaki yapidan kaynaklaniyordu, daha detayli inceleyecegiz.


# server_names_hash_bucket_size controls the maximum length of a virtual host entry (ie the length of the domain name).
In other words, if your domain names are long, increase this parameter.
You need to add this flag in the http context:
http {
    server_names_hash_bucket_size 64;
    ...
}

# cache'te key belirtince query string'le baslayan parametreleri de cache'e dahil ediyor.

If you want no www.* traffic to your server;
server { 
server_name ~^www\.(?<domain>.*)$; return 301
$scheme://$domain$request_uri; 
}

* set $variable value;
Context:    server, location, if
Sets a value for the specified variable. The value can contain text, variables,
and their combination.

### Modules
#### http_map_module
Creates a new variable whose value depends on values of one or more of the
source variables specified in the first parameter.
Parameters inside the map block specify a mapping between source and resulting
values.

#### http_rewrite_module

* dizindeki dosyalari index'siz siralama (Apache'deki option -indexes)


location /path_of_your_directory{ 
   ... ( some other lines )
   autoindex on;
   ... ( some other lines )
}

* Hata: a client request body is buffered to a temporary file
/var/lib/nginx/body/
Cozum: client_body_buffer_size'i arttirmak.
bu parametreyi nginx.conf'ta belirtmediginde default olarak 16K degerle
calisiyor.
