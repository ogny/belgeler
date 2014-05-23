* uzak ftp'deki tum dizinleri recursive olarak kopylamak icin;
lftp -u kullanici_adi ip_adresi
mirror -c dizin

* cpanel'de ssh login olmayan ortamda sftp'yi kullanabiliyoruz;
sftp kullanici_adi@host_ip:/dizin_adi/dosya_adi .
