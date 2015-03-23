================
Vmware ESXI 5.5
================

:date: 2015-02-11

#. Disk klonlarken dikkat edilecek;
Emrah'in notta duzeltme;
http://www.emrah.com/notlar/vmware_notlari.txt 
full path verilirken /vmfs/volumes/DatastoreX olarak degil mount edildigi uuid'yi veriyoruz.

#. Vcenter'da ssh'i acma
Vcenter'da Configuration > Security Profile > Services'in sag ustte properties'te ssh var.

#. Sanal sunucuya  disk eklendiginde scsi bus'ları rescan ettirmek;

..code:: sh

    for i in /sys/class/scsi_host/*; do echo "- - -" > $i/scan; done


Yonetim
-------

#. Bir sunucu klonlandiginda, o sunucunun kok diski ve varsa ek diskinin
bulundugu storage'tan degil, yeni bir storage'tan disk bolumu ekliyor. HA
acisindan uygun

#. thin provision ile olusturulmus bir diskin boyutunu istedigimiz zaman
  arttirabiliyoruz, sunucuyu kapatip bunun icin settings'ten ilgili diski secip
  bar'dan arttiriyoruz.

#. 5.5.0'da interface eklerken interface'in turunun VMXNET50 olmasina dikkat
   et.


