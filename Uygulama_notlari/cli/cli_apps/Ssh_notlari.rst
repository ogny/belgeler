* Uzak sunucudaki dosyanin sonuna yerel dosyadan ekleme::

    ssh user@remoteserver "/sbin/cat >>remotefile" <localfile

* Sorun: Terminale devamli asagidaki uyari basiliyorsa::

    fatal: Read from socket failed: Connection reset by peer [preauth]

* Cozum: ssh_config dosyandaki Cipher'deki comment'i kaldir;

`Kaynak:note.id.lv <http://www.note.id.lv/2014/12/ssh-issues-read-from-socket-failed.html>`_

* Passphrase olusturulmus key'in passphrase'ini kaldirma::

    ssh-keygen -p

Choose keyfile Enter the old passphrase and the new passphrase (enter nothing). 

`Kaynak:stackoverflow <http://stackoverflow.com/questions/112396/how-do-i-remove-the-passphrase-for-the-ssh-key-without-having-to-create-a-new-ke>`_
