SSH Notlari
===========

* ekli ozel anahtarlari silme, listeleme::

    ssh-add -D
    ssh-add -l

* ozel anahtar olusturma::

   ssh-keygen -C "$(whoami)@$(hostname)-$(date -I)"  -t rsa -b 2048 -f ~/.ssh/$(hostname)

* ozel anahtardan genel anahtar olusturma::

    ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.pub

* genel_anahtar'in fingerprint'ini gorme::

    ssh-keygen -lf ~/.ssh/id_rsa.pub

* ozel anahtari 'i X'in oldugu ortamda hafizada tutma::

    xclip -sel clip < ~/.ssh/genel_anahtar

* uzak sunucuya ssh genel anahtari kopyalama::

    ssh-copy-id -i dosya_adi.pub "kullanici@12.12.12.12 -p2222"

* Warning: the ECDSA host key for differs from the key for the IP address::

    ssh-keygen -R <IP_address>

* ozel anahtar passphrase degistirme::

    ssh-keygen -f <Ozel_anahtar> -p

* Uzak sunucudaki dosyanin sonuna yerel dosyadan ekleme::

    ssh user@remoteserver "/sbin/cat >>remotefile" <localfile

* Sorun: Terminale devamli asagidaki uyari basiliyorsa

    fatal: Read from socket failed: Connection reset by peer [preauth]

* Cozum: ssh_config dosyandaki Cipher'deki comment'i kaldir;

[Kaynak:note.id.lv](http://www.note.id.lv/2014/12/ssh-issues-read-from-socket-failed.html)

* Passphrase olusturulmus key'in passphrase'ini kaldirma::

    ssh-keygen -p

Choose keyfile Enter the old passphrase and the new passphrase [enter nothing]. 

[Kaynak:stackoverflow](http://stackoverflow.com/questions/112396/how-do-i-remove-the-passphrase-for-the-ssh-key-without-having-to-create-a-new-ke)

* know_hosts'a ekli pubkey'i silme
```
ssh-keygen -f "/root/.ssh/known_hosts" -R  <ip_adresi>
```

* uzak makinada komut calistirmak
```
ssh <uzak_makina> -t "komut"
```

* config dosyasini bypass ederek default tanimla baglanma
```
ssh -F /dev/null <ip_adresi>
```

* key olusturma
```
ssh-keygen -C "$(whoami)@$(hostname)-$(date -I)" -b 2048
```
