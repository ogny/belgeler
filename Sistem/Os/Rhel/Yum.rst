Yum
===

1. Repo'lardan paket indirme;

* Kurmadan::

    yum install yum-plugin-downloadonly
    yum install --downloadonly --downloaddir=<dizin_adi> <paket_adi>

        * --downloaddir= belirtilmezse /var/cache/yum/ altina indirir.
        * yum groupinstall ve yum groupinfo ile calismaz.

*  Kurduktan sonra::

    yum install yum-utils
    yumdownloader <paket_adi>

        * Indirilecek dizin belirtilmezse bulunulan dizine indirir.
        
* Bagimliliklari cozme::

  yum deplist <paket_adi>

yum-utils ile geliyor::

   repoquery --requires --recursive --resolve <paket_adi>

* Hata::
    There are unfinished transactions remaining. You might consider running
    yum-complete-transaction first to finish them

    yum-complete-transaction --cleanup-only

* spesifik bir repo'dan paket sorgulama::

    yum --disablerepo="*" --enablerepo="epel" search <paket_adi>

* multi architecture'yi iptal etme /etc/yum.conf'a ekle::

  exclude=*.i386 *.i686

