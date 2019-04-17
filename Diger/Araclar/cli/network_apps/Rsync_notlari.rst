Rsync Calismasi
===============

:date: Prş 26 Şub 2015 15:23:22 EET
:comments: true
:categories:
:tags:
:Author: Orkun Gunay

```

/usr/bin/rsync \
    --archive \
    --compress \
    --owner \
    --group \
    --perms \
    --recursive \
    --remove-source-files \
    /home/deployer/dizin/ \
    /home/deployer/

```

#. --existing ile sadece varolan dosyalari guncelliyoruz::

    rsync -avzuhe "ssh -l kullanici" IP_ADRESI:/etc/nginx/ etc/nginx/ \
    --existing

#. --exclude ile hedeften guncellenmesini istemedigimiz klasorleri exclude
   edebiliyoruz::

    rsync -avzuhe "ssh -l kullanici" IP_ADRESI:/etc/nginx/ etc/nginx/ \
    --exclude='sites-enabled'

rsync -aAXvP --exclude={"/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*",\
"/media/*","/lost+found","/etc/sysconfig/network-scripts/*","/etc/network/interfaces"} \
kaynak/* /hedef/

#. yedek almak icin::

    rsync -a --verbose --recursive --links --perms --executability \
    --owner --group --time

#. linklenmis dosyalarin gonderilmesi rsync over ssh'le dosya gonderilirken -l
   secenegi kullanilamiyor, hata veriyor.

#. rsync'le kaynaktaki dosyalari hedefe tasimak icin; (kopyalamadan)::

    rsync --remove-source-files --compress-level=1 \
    -av  kaynak_makina:/*.dosya_uzantisi hedef_makina:/dizin

#. inplace eklenecek

#. progress bar::

    --progress

#. Rsync kesildikten sonra kopyalamanin kaldigi yerden devam etmesi::

    rsync --append --progress --partial -azvv <yerel_dizin> \
    <kullanici_adi>@<uzak_sunucu>:<uzak_dizin>

#.  Append'de dosyanin boyutuna bakiyor, gonderilecek yerdekiyle esit veya
    ondan kucukse gondermeyi atliyor.

#. debug etmek icin `--debug=all4`

#. mv ile alinan `preserving permissions for ... Operation not permitted` hatasindan kurtulmak::
rsync -avh --no-owner --no-group --remove-source-files "$backupfile" "$destination"

#. rsync ile silme::
    rsync -d --delete <bos_dizin>/ <silinecek_dizin>/

#. find ile bulup rsync'le gonderme::
rsync -avz --files-from=<(find . -type f -iname "*.mp3" -mtime -1) ./ /hedef
rsync -zavh --files-from=<(ssh <kullanici>@<IP> 'find /backups/* -mtime -1 -exec ls -d {} \;') <kullanici>@<IP>:/ b/


