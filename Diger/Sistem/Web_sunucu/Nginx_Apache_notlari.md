* Centos6 Nginx ssl kurulum 


Domain'in Dns kaydi olusturulur.

lbweb ve cacheweb sunucusunda;
cd /etc/nginx/sites-available
cp site site1
vim site1
:%s///gc
cd ../sites-enabled
ln -s ../sites-available/site1

yedekler alinir:
rsync -avzuhe "ssh -l kullanici adi -p port" :/etc/nginx/sites-available/ etc/nginx/sites-available

cacheweb sunucusunda;
Yukaridaki islemlerin aynisi
mkdir /var/www/site1
chown KullaniciAdi: /var/www/
cd /etc/apache2/sites-available/
Yukaridaki islemlerin aynisi
cp site-ssl site1-ssl
Yukaridaki islemlerin aynisi


* Yeni bir cache alani olusturulur;
vim /etc/nginx/conf.d/cache.conf
proxy_cache_path  /var/cache/nginx_cache levels=1:2 keys_zone=one:64m max_size=8192m inactive=600m;

yedekler alinir:
rsync -avzuhe "ssh -l kullanici adi -p port" :/etc/apache2/sites-available/ etc/apache2/sites-available/

ftp ve mysql parolalari asagidaki dosyaya kaydedilip oradan kopyalanir
repo'ya degisen dosyalar gonderilir.
git add -A
git commit -m ""
git push

pure-pw useradd -u KullaniciAdi -d /var/www/
pure-pw mkdb

mysql -u root -p
create database ;
CREATE USER ''@'localhost' IDENTIFIED BY '';
GRANT ALL privileges on .* TO ''@'localhost';
flush privileges;

sunucular dosyasi git repo'suna atilir.
servisler yeniden baslatilir;
/etc/init.d/nginx configtest
/etc/init.d/nginx restart
apache2ctl configtest
/etc/init.d/apache2 restart

cd /home/KullaniciAdi/htaccess
htpasswd -s htpasswd 

yedeklerin alindigi repo'ya degisen dosyalar gonderilir.
git add -A
git commit -m ""
git push
