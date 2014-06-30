* ekli ozel anahtarlari silme
ssh-add -D

* ozel anahtardan genel anahtar olusturma
ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.pub
 
* genel_anahtar'in fingerprint'ini gorme
ssh-keygen -lf ~/.ssh/id_rsa.pub

* ozel anahtari 'i X'in oldugu ortamda hafizada tutma;
xclip -sel clip < ~/.ssh/genel_anahtar

* uzak sunucuya ssh genel anahtari kopyalama
ssh-copy-id -i dosya_adi.pub "kullanici@12.12.12.12 -p2222"
