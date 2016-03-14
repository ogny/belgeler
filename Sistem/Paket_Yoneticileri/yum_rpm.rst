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

* paketlerin bagimliliklarini gorme::

    yum deplist <package>

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

#. paketleri boyutlarina gore siralama::

    rpm -qa --queryformat="%{NAME} %{SIZE}\n" | sort -k 2 -n

#. `yum reinstall <paket_adi>`

#. Group paket komutlari::

   yum grouplist --> Lists installed groups and groups that are available for installation.
   yum groupinfo groupname --> Displays detailed information about a group.
   yum groupinstall groupname  --> Installs all the packages in a group.
   yum groupupdate groupname  --> Updates all the packages in a group.
   yum groupremove groupname --> Removes all the packages in a group.
