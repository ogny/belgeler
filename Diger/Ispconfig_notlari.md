* ftp parolalarini bulma; (pure-ftpd-mysql kullaniyor)

use dbispconfig;
SELECT password FROM ftp_user WHERE active = 'y' AND server_id = '1' AND username="ftp_kullanicisi";
kaynak: /etc/pure-ftpd/db/mysql.conf
