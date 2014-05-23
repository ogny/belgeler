# paylasimdan dosya gondermek
    smbclient -A samba.crt \\\\192.168.1.130\\AnaPaylasim\\  -c "cd AltPaylasim/AltPaylasim; prompt off; put seckintest; exit"
    samba.crt
    """
    username=seckin
    password=seckin123
    domain=EV
    """

# samba diski mount etmek
    smbmount //192.168.1.130/AnaPaylasim /root/samba -o user=seckin,domain=EV

# sambada actigin her kullanicidan sonra servisi restart etmek istemiyor isen, paylastigin klasorde kullanici ismini %U ile belirt

# Paylasimla ilgili sorunlari smb.conf'ta asagidaki gibi yaparak cozebiliyoruz;
[dizin_adi]
        path = /dizin_yolu
        read only = No
        public = yes
        browsable = yes
        writable = yes
        create mask = 0777
        directory mask = 0777
