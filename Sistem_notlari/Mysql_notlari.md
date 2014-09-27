### Kurulum
```
apt-get install mysql-server
```
* veri tabanina yeni tablo ve deger girme;
` mysql 5.5 surumunde "The used command is not allowed with this MySQL version"
diyor, cozum asagidaki gibi prompt'u baslatmakta;`
```
mysql --local-infile=1 -uroot -p
```

* Bir kullaniciya root haklari vermek ;
UPDATE mysql.user SET Grant_priv='Y', Super_priv='Y' WHERE User='me';

* Uzerinde yapilan sorgulari gormek;
show processlist;

* sorgularin yavasladigi farkedilirse /tmp altinda ne kadar buyuklukte dosyalar
olusturmaya calisiyor, onu arastir.

* MyISAM engine'de table lock var, ornegin bir select processi tabloyu kilitlerse, diger update process'leri tum select process'lerinin bitmesini bekler, bu durum okumada hiz yazmada yavaslik dogurur.
Ancak tam tersi oldugunda sitenin gelmesi gittikce gecikir, bunu onlemek icin update process'leri icin low latency belirtebiliyoruz.

* Yeni kullanici ve veri tabani yaratip o kullaniciya o veri tabani uzerindeki tum haklari vermek;
CREATE USER 'kullanici_adi'@'localhost'  IDENTIFIED BY 'PAROLA';
create database veri_tabani_adi;
GRANT ALL privileges on veri_tabani_adi.* to 'kullanici_adi'@'localhost';
flush privileges;

* Kullanicinin parolasini degistirmek;
use mysql;
update user set password=PASSWORD("PAROLA") where User='KULLANICI_ADI';

* Belli tablolara gore sorgu yapip tablo icindeki sutun degerlerini guncellemek;
update tablo_adi set sutun_adi=yeni_deger where tablo='tablo_adinin_ait_oldugu_deger'

* Mysql check Repair all tables in one go
mysqlcheck -u root -p --auto-repair --check --optimize --all-databases

* mysql tablosundaki parolayi degistirme
 mysql> select * from wp_users;
+----+------------+------------------------------------+
| ID | user_login | user_pass                          |
+----+------------+------------------------------------+
|  1 | admin      |				       | 
|  2 | 		  |				       | 
+----+------------+------------------------------------+
mysql>  update wp_users set user_pass='' where user_login='admin';

* kullanicinin sahip oldugu yetkilere bakmak
SHOW GRANTS FOR 'kullanici'@'sunucu';
* kullanici parolasini guncellemek;
SET PASSWORD FOR 'user-name-here'@'hostname-name-here' = PASSWORD('new-password-here');


dump ve restore
mysql -u root -p < dosya.sql
mysqldump -u root -p veritabani > veritabaniadi.sql

* linux ilk kurulumda default InnoDB engine geliyor
* verilmis izinleri geri alma;
`REVOKE ALL PRIVILEGES, GRANT OPTION FROM user [, user]…`
`REVOKE ALL PRIVILEGES ON phpmyadmin.* FROM 'phpmyadmin'@'localhost';`
