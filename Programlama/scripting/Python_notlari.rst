====================
``Python Tutorial``
====================

:date: 2015-01-31

String'ler ve Listeler [*]_

#. indexlenebilir ve bolunebilir

#. Her bolme sonrasi, yeni elemanlarla yeni bir liste olusur

#. her ikisi de concatenate edilebilir. iki veya daha fazla string art arda
   boslukla yazilirsa otomatik concatenate edilir. (bunu uzun string'lerin
   okunmasi icin bolmede kullanabilirsin.)


#. liste elemanlari degistirilebilir, string'ler degistirilemez. indisi
   ile degistirilir, append metoduyla sona yeni eleman eklenir.

   #. String'leri degistirmek icin yeniden olusturmak gerekir;

#. len her ikisini de karakter sayisini verir.

#. listeler ic ice barindirilabilir.

#. tirnak icindeki bir ifadeyi disaridan da cagirmak icin " dosya %s" %
   atanacak_dosya

#. return'u func. disinda kullanamiyoruz.

String formatting [*]_

#. Bash'teki positional arguman gibi arguman .format sonuna arguman verilir. (parametre belirtmek sart degil, sirali takip ediyor)

..code:: python

    str.format

    "The sum of 1 + 2 is {0}".format(1+2)

#. karmasik positional'lar icin keyword de atayabilirsin;

..code:: python

    print 'This {food} is {adjective}.' \
    .format(food='spam', adjective='absolutely \
    horrible')

#. %s ile string'i %d ile integer'i map'lenir.

#. how do you convert values to strings? Luckily, Python has ways to convert
   any value to a string: pass it to the repr() or str() functions.

#. dict'lerde giris cikis duzenli olmuyor. Neden burada siranin garantisi yok?
   insertion sirada degil, alphabetic (key order'de da degil). Neden? dict
   aslinda hashtable (burada hashmap'e karsilik geliyor)
   key'i hash'e veriyorum, bana bir sayi veriyor 35
   2.key'i hash'e veriyorum, bana bir sayi veriyor 75
   bellekte tamamen random duruyor. Bunun avantaji sirali arama yapmiyorum,
   nokta atisiyla gidiyorum. for'la in not in yaparak __contains__ metoduyyla
   direk bulabiliyorsun. sirali yazmak istiyorsan std. kutuphanede yok,
   on-the-fly yazabilirsin.

#. ftp'ye baglanma

..code:: python

  import ftplib
  ftp = ftplib.FTP()
  ftp.connect('<ip_adresi>', 21)
  '220 (vsFTPd 2.2.2)'
  ftp.login('<kullanici_adi>', '<parola>')
  '230 Login successful.'
  ftp.voidcmd('TYPE I')
  '200 Switching to Binary mode.'
  ftp.size('dosya_adi')


Docstring Conventions [*]_

#. 

Kaynaklar
---------

.. [*] py tutorial: https://docs.python.org/2/tutorial/introduction.html#using-python-as-a-calculator

.. [*] py 2.4 tutorial: https://docs.python.org/2.4/lib/typesseq-strings.html

.. [*] Docstring Conventions: https://www.python.org/dev/peps/pep-0257/
