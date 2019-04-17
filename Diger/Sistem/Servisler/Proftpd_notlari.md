SFTP ile erisim;
Derlenecek uygulamalar; openssl
http://www.openssl.org/source/
Derleme adimlari;
http://www.inkatel.com/new/textos/sistemas/apache-ssl-php-mcrypt-mod_perl/node3.html
OPENSSLDIR: "/usr/local/ssl"

gerekli paketler;
make gcc zlib-devel (43 paket iceriyor)

openssl-1.01e kurulu, kaldirilamiyor. 

cp <path>/proftpd-1.3.5b/contrib/dist/rpm/proftpd.init.d /etc/init.d/proftpd
chmod +x /etc/init.d/proftpd
chkconfig --add proftpd
chkconfig --level 35 proftpd o n

* uzak ftp'deki tum dizinleri recursive olarak kopylamak icin;
lftp -u kullanici_adi ip_adresi
mirror -c dizin
* yereldeki tum dizinleri uzak ftp'ye recursive olarak kopylamak icin;
mirror -R dizin

* cpanel'de ssh login olmayan ortamda sftp'yi kullanabiliyoruz;
sftp kullanici_adi@host_ip:/dizin_adi/dosya_adi .


