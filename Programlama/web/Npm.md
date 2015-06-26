Nodejs
----

* Ubuntu'da nodejs paketini kur.
* Her projede kullanilacak temel komutlari global olarak save et.
  (grunt,bower,karma)
* temel komutlari initialize et, temel file'lari olusun. (bower.json,
  package.json vd.)
* css ve js'te calisacak kodu class olarak belirterek ekliyorsun.

#### Npm
paketleri bulundugun path'e gore kuruyor.
best practise'i; proje dizininde kur, gitignore'a yaz.
* pip'le mukayese ettiginde  
    - dependency'leri hiyerarsik gostermesi guzel.
    - requirements.txt'yi elle yazmak zorundaydin, burada degilsin. `--save`
      ile verdiginde package.json'a yaziyor.

Javascript'teki initjs ile package.json yaziyorsun (python'daki setup.py)

#### Bower

hazir js kutuphanelerini indirme/versiyonlamayi saglar.
* Ayni paket adiyla npm'le ve bower'la kurdugun paketlerin fonksiyonlari
  birbirlerinden farklidir.

##### wiredep
Koduna bower ile kurulabilen kutuphaneleri eklemek icin kullanilir.
[bower](https://www.npmjs.com/package/wiredep)

#### Grunt
otomatizasyon icin kullaniliyor. Ozellikleri;

* Calistirmak icin Gruntfile'a ihtiyac var.
    - bu bir kere olusturulacagindan, hazir template'leri kullanarak
      olusturabiliriz. bunun icin `grunt-init`'i kuruyoruz.

* sass, {less} vs.'i css'e ceviriyor.

Kurulum icin 3 paketi var;
`grunt`, `grunt-cli`, `grunt-init`, `grunt-wiredep`

Ek olarak grunt-contrib paketlerini kurmak gerekiyor.

* Fonksiyonlar;
    - jshint (hatalari engeller)
    - uglify (url'leri vs. duzenler)
    - concat (sikistirma gibi)
    - qunit (testler)
    - watch (tum fonksiyonlari her degisiklikte tekrar calistirir)

* Grunt olmasaydi, bunlari manuel olarak, regex'lerle calistiracaktik.

#### Bootstrap 3

css framework'u, fluid container destekler (verdigin size'a gore wide yapma)

#### Karma JS test runner

Jenkins'te karmayi calistir, js'ler patliyorsa deploy cikma.

