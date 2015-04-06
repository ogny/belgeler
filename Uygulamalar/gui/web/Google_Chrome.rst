=========
Chromium 
=========

* Sekmeleri toplu olarak manipule edebiliyoruz
Sekmeleri ctrl'ye basip mouse ile secerek grupluyoruz, grupladigimiz sekmeleri
topluca kapatabiliyor, baska bir window'a tasiyabiliyoruz.

* chromium diger cihazlari inspect mode'da acamiyor, bunun icin google-chrome-stable
kurmak gerek; paketi google'dan indirip kurmadan once depo'dan
libappindicator1'u kuruyoruz.

* vimium find mode'da kelime tarayip enter'a bastiginda system-clipboard'a
atiyor

* google-chrome-stable kurulumu sonrasi eksik library'nin eklenmesi;

.. code:: sh

   sudo ln -s /lib/x86_64-linux-gnu/libudev.so.1.3.5 /usr/lib/libudev.so.0

`Kaynak:askubuntu <http://askubuntu.com/questions/369310/how-to-fix-missing-libudev-so-0-for-chrome-to-start-again>`_

Eklentilere ozel yapilandirma
=============================

* Stylebot manuel sync: 
  Stylbot'a sync ozelligi gelmediginden, store db'nin oldugu eklentinin tum dizinini elle sync ediyorum dizinin yolu;

.. code:: sh

   ~/.config/google-chrome/Default/Local Extension Settings/oiaejidbmkiecgbjeifoejpgmdaleoha


