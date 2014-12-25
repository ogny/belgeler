* Uzak sunucudaki dosyanin sonuna yerel dosyadan ekleme

.. code:: sh

        ssh user@remoteserver "/sbin/cat >>remotefile" <localfile

* Sorun: Terminale devamli asagidaki uyari basiliyorsa;

fatal: Read from socket failed: Connection reset by peer [preauth]

.. code:: sh

           fatal: Read from socket failed: Connection reset by peer [preauth]

* Cozum: ssh_config dosyandaki Cipher'deki comment'i kaldir;

`Kaynak:note.id.lv <http://www.note.id.lv/2014/12/ssh-issues-read-from-socket-failed.html>`_

