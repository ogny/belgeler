#### usage:

* By default tmpreaper, will delete files based on "Access Time" (olusturulan
  tarih --atime). You can use "-m" option to tell tmpreaper to delete files
  based on "Modification time" (degistirilen tarih --mtime).
* To remove files which are 5 days older, use “5d” as timespec
* You can also use the following characters for time_spec parameter
* Remove all File Types using -a Option
* "-t" ile dry-run yap
* "-f" ile force delete
* en guzel ozellik, --protect ile dosyalari koru, kullanimi;
    - henuz recursive yapamadim, ilk bulduguyla match olup birakiyor.
    - dizin/dosya koruyabiliyor.

#### Ornekler:
* son 5 dakikada, Downloads altindaki her seyi sil
```bash
$ tmpreaper -a 5m ~/Downloads
```

#### Tarih parametreleri
d – for days
h – for hours
m – for minutes
s – for seconds

As long as there are files present inside a subdirectory, it won't get removed.
You can use a non-writable, self-owned file, perhaps named ".tmpreaper", or, if
you are su, a file that has the ext2fs immutable attribute set, to keep a  sub‐
directory  from being deleted.  Of course, you could just as easily use use the
--protect option to obtain the same result.

