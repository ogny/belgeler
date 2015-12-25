=================
Rpm Centos Calisma
=================

:date: Çrş 18 Şub 2015 11:58:23 EET
:comments: true
:categories: 
:tags: 
:Author: Orkun Gunay


#. kurulu paketlerde arama yapmak::

    rpm -qa |grep <paket_adi>

#. paket hakkinda bilgi almak (apt-show)::

    rpm -qi <paket_adi>

#. paket kaldirma::

    rpm -e  <paket_adi>

#. paket kurarken birbirine bagimliliklari olanlari ayni satirda yaz::

    rpm -ivh <paket_adi1> <paket_adi2>

#. paket kurarken conflict'leri dikkate almamak icin::

    --force Same as using --replacepkgs, --replacefiles, and --oldpackage.

#. lokal'de bulunan bir paketin dependency check'i::

    rpm -qpR  <paket_adi>

#. dosyalari acma::
    mkdir acilacak_dizin ; cd acilacak_dizin
    rpm2cpio <paket_adi.rpm> | cpio -idmv

