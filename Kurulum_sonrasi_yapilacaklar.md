## Kurulum sonrası Yapılacaklar  
  
Bu belgede herhangi bir amaçla kurulan Debian işletim sisteminde, kurulumdan  
sonra yapılacak ilk işlemler adım adım belirtilecektir. Bu belgenin  
hazırlanmasında temel alınan aşağıdaki belgeler, belirtilen belge numarasına  
göre referans gösterilecek, referans belgelerinde olmayan işlemler, işlem  
sırasına göre eklenecektir.  Tüm işlemler Digital Ocean'ın Wheezy x86_64  
droplet imajı üzerinde test edilmistir.  Sisteme sonradan eklenen veya üzerinde  
değişiklik yapılan tüm yapılandırma dosyaları git repo'suna eklenir.   
Git repo'sunda her seferinde kullanıcı adı, parola girmemek için;  
git remote set-url origin repo_adı   
  
* Referanslar:  
1) http://emrah.com/notlar/debian_kurulum_notlari_wheezy.txt  
2) http://www.serdaraytekin.com/docs/os/debian/debian-apt-pinning.html  
3) http://www.serdaraytekin.com/docs/os/debian/debian-cron-update.html  
4) http://emrah.com/notlar/ssh_notlari.txt  
5) http://emrah.com/notlar/vim_notlari.txt  
6) http://www.serdaraytekin.com/docs/os/debian/sss/  
  
### Erişim Güvenliği  
Ref.1 SSH ayarları (Servis yeniden başlatılmaz.)  
  
### Paket Yöneticisi Ayarları:  
Ref.1 Kurulum sonrası ilk ayarlar. (Backports ve Multimedia depolari eklenecek)  
  
* Sistem güncelleme ve yeni paketlerin kurulumu:  
Ref.1 Güncelleme - İlk aşamada yüklenecek paketler   
Eklenenler: cryptsetup encfs  
Default paketlerden silinecekler.   
  
/etc/passwd   
/etc/adduser.conf  
/etc/ssh/sshd_config yedegi alınır.  
  
* Root parolasını değiştirme  
passwd  
/etc/passwd yedegi alınır.  
  
  
### Kullanıcı Oluşturma:  
Kullanıcıyı oluştururken istediğimiz gruplara eklemek için;  
vim /etc/adduser.conf  
DSHELL=/bin/zsh  
EXTRA_GROUPS="dialout cdrom floppy audio video plugdev users sudo fuse netdev"  
ADD_EXTRA_GROUPS=1  
adduser orkung  
/etc/adduser.conf  yedegi alınır.  
Ev dizinini şifreleme  
  
### şifreli dizin üzerinde çalışma  
Dizini açma  
cryptsetup luksOpen /dev/sda2 map1  
/dev/mapper/map1  
mount  /dev/mapper/map1 /mnt  
cryptsetup luksOpen /dev/sda2 map1  
  
  
Kullanıcıya giriş yapılacak açık anahtar yüklenir.  
ssh-copy-id -i açık_anahtar kullanıcı_adı@IP_adresi  
  
SSH servisi yapılandırma dosyasına göre çalışması için yeniden başlatılır.   
/etc/init.d/ssh restart  
/etc/ssh/sshd_config yedegi alınır.  
  
### Kullanıcı Ayarları:  
su -l kullanıcı_adı  
mkdir Git_Repolari  
cd Git_Repolari  
git clone https://ogny@bitbucket.org/ogny/system_configuration.git  
cd system_configuration  
rsync -avh --exclude .git --exclude X11 --exclude etc --exclude .gitconfig ./ ~/  
  
Ref.5 Vundle  
cd ..  
git clone https://github.com/gmarik/vundle.git  
cp -rp ~/vundle/* ~/.vim/bundle  
  
#### Masaüstü ortamı  
* X11 kurulumu   
* i3wm kurulumu ( -t wheezy-backports i3 )
  
# Uygulamalar  
*  weechat'i emrah'tan al.  
*  bitlbee i emrah'tan al.  
*  pulseaudio emrah'tan ref.'te  
#### Repo'lardan kurulacak paketler  
```  
sudo apt-get install tig ranger whois dosfstools dnsutils dos2unix ffmpeg  
apt-listchanges wicd-curses wireless-tools parted curl systemd pandoc rtmpdump  
dstat mtr-tiny sshuttle ca-certificates lxc lxctl debootstrap rfkill  
coreutils gnupg pass sshpass git-flow git-extras apache2-utils python-pelican
autofs smartmontools ack-grep acct
* istege bagli; html2text transmission-cli gawk nodejs-legacy npm
* X11 kuruluysa yukaridakilere ek olarak;  
sudo apt-get install chromium scrot feh zathura redshift suckless-tools wodim  
fontconfig-infinality pepperflashplugin-nonfree unclutter numlockx imagemagick xautolock i3status dunst compton j4-dmenu-desktop && sudo
dpkg-reconfigure x11-common 
```  
* ubuntu kurulduysa 
    ** kaldirilacaklar;
       whoopsie  apport oneconf python3-apport python3-problem-report libcups2
       gnome-keyring
    ** kurulacaklar; 
       python-software-properties
       acpi
Not: Ubuntu server security updates'i otomatik yapmak icin popcorn kullaniyor,
incelenecek
  
* makinanin dis ip'den cikisi varsa nagios-nrpe-server (--with-recommends ile)  
* pass ve gnupg organize et (http://www.passwordstore.org/)  

* ascii screen saver;
```
cd /tmp
wget http://www.robobunny.com/projects/asciiquarium/asciiquarium.tar.gz
tar -zxvf asciiquarium.tar.gz
cd asciiquarium_1.1/
sudo su
cp asciiquarium /usr/local/bin
chmod 0755 /usr/local/bin/asciiquarium
perl -MCPAN -e shell
install Term::Animation
```

#### task sync ozelligiyle kullanilacaksa  
```  
sudo apt-get install cmake uuid-dev libgnutls-dev build-essential -y  
```  
#### Samba ile agdaki bir diske erisilecekse;
```  
sudo apt-get install cifs-utils
vim ~/.smbcredentials 
username=<samba_username>
password=<samba_password>
:wq
sudo vim /etc/fstab 
//<ag_cihazi>/<dizin>     /<baglanacak_dizin>           cifs uid=<linux_username>,credentials=/home/<linux_username>/.smbcredentials,iocharset=utf8,sec=ntlm 0       0
:wq
sudo mount -a
```  

#### akilli telefon baglama
```
mtpfs libfuse-dev libmad0-dev jmtpfs
  
#### github'tan kurulacak uygulamalar  

* cheat
* dircolors-solarized
* fasd
* geeknote
* j4-dmenu-desktop
* monaco-font
* tmux-colors-solarized
* turkish-deasciifier
* wped
* zsh-syntax-highlighting
