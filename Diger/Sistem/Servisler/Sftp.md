groupadd sftponly

useradd -d /mnt/john -s /bin/false -g sftponly john

chown root: /mnt/john ; chmod 0755 /mnt/john
Remote working directory: /mnt/john
 # gpasswd -a USER sftponly

yazma izni verilecek;
sahipligi duzenlenecek
alt dizinlere chmod'la 700 ver
chown ver

