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
        

