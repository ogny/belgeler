### Genel
#### Oncelikle yapilacaklar
* apt-get install sudo
* user'a passwordless sudo yap `visudo`
```
orkung ALL=(ALL) NOPASSWD: ALL
```
* root ve user'in default login shell'ini zsh yap. Kullaniciya gec.
```
sudo apt install zsh tmux vim-nox 
vim /etc/passwd
su - orkung
mkdir -p ~/Git_Repolari/kisisel/public/
mkdir -p ~/Git_Repolari/kisisel/private/
```
* sudo vim /etc/apt/sources.list 
```
deb http://httpredir.debian.org/debian jessie main contrib non-free
deb-src http://httpredir.debian.org/debian jessie main contrib non-free
deb http://httpredir.debian.org/debian jessie-updates main contrib non-free
deb-src http://httpredir.debian.org/debian jessie-updates main contrib non-free
deb http://security.debian.org/ jessie/updates main contrib non-free
deb-src http://security.debian.org/ jessie/updates main contrib non-free
# backports repo
deb http://httpredir.debian.org/debian jessie-backports main contrib non-free
deb-src http://httpredir.debian.org/debian jessie-backports main contrib non-free
# Multimedia repo
deb http://www.deb-multimedia.org jessie main non-free
deb-src http://www.deb-multimedia.org jessie main non-free
```
* Uygulama Repo'lari
```
echo "deb http://code.bitlbee.org/debian/master/jessie/amd64 ./" \
| sudo tee /etc/apt/sources.list.d/bitlbee.list
echo "deb http://downloads.hipchat.com/linux/apt stable main" \
| sudo tee /etc/apt/sources.list.d/hipchat.list
echo "deb http://dl.google.com/linux/chrome/deb/ stable main" \
| sudo tee /etc/apt/sources.list.d/google_chrome.list
```
* X kurulumu bittiginde startx ile login ol
```
sudo apt install --install-recommends xorg
sudo apt install -t jessie-backports --install-recommends i3 

```
* Guncelleme
```
sudo apt update && \
sudo apt-get -dy dist-upgrade && \
sudo apt-get autoclean && \
sudo apt dist-upgrade && \
sudo apt-get autoremove
```
* Silinecekler
```
sudo apt purge nfs-common rpcbind installation-report \
reportbug nano tasksel tasksel-data task-english os-prober
```
```
sudo apt install -t jessie-backports --install-recommends owncloud-client
sudo apt install --install-recommends linux-headers-$(uname -r|sed 's,[^-]*-[^-]*-,,') \
virtualbox virtualbox-qt
sudo apt install ack dkms google-chrome-stable hipchat git git-flow git-extras \
wicd wicd-daemon wicd-curses ranger whois dosfstools dnsutils dos2unix \
apt-listchanges curl pandoc dstat mtr-tiny sshuttle ca-certificates rfkill \
coreutils gnupg sshpass apache2-utils autofs acct scrot feh redshift \
suckless-tools unclutter numlockx xtrlock dunst xbindkeys openvpn tpp
```
* Manuel
  - rxvt-unicode-256color_9.19-1.1_amd64.deb
  - pfSense-udp-1194-tyildiz-config.zip
  - 1a23_setup_sst53.exe
  - chefdk.tar.bz2
  - lokal_user_chefdk_for_jekyll.tar.bz2
  - diger_git_repolari.tar.bz2

#### Networking 
* wicd'te kullandigin baglanti> sag ok >"automatically connect to this network"
sudo apt-get install tig ranger whois dosfstools dnsutils dos2unix ffmpeg  

* Wine32 kurulumu
```
sudo dpkg --add-architecture i386 && sudo apt update
sudo apt install wine
sudo apt install \
      wine-development/jessie-backports \
      wine32-development/jessie-backports \
      wine64-development/jessie-backports \
      libwine-development/jessie-backports \
      libwine-development:i386/jessie-backports \
      fonts-wine-development/jessie-backports
```
* i386 paketlerini kaldirmak istersen link; 
[How to setup, and remove, multiarch in Debian Jessie](http://www.sharons.org.uk/amd64.html)
  
### Paket Yöneticisi Ayarları:  
Ref.1 Kurulum sonrası ilk ayarlar. (Backports ve Multimedia depolari eklenecek)  
  
* Sistem güncelleme ve yeni paketlerin kurulumu:  
Ref.1 Güncelleme - İlk aşamada yüklenecek paketler   
Eklenenler: cryptsetup encfs  
Default paketlerden silinecekler.   
  
/etc/passwd   
/etc/adduser.conf  
cp /etc/ssh/sshd_config{,.bck}
  
* Root parolasını değiştirme  
passwd  
cp /etc/passwd{,.bck}
  
  
### Kullanıcı Oluşturma:  
Kullanıcıyı oluştururken istediğimiz gruplara eklemek için;  
vim /etc/adduser.conf  
DSHELL=/bin/zsh  
EXTRA_GROUPS="dialout cdrom floppy audio video plugdev users sudo fuse netdev"  
ADD_EXTRA_GROUPS=1  
adduser orkung  
cp /etc/adduser.conf{,.bck}
* centos'ta
useradd -md /home/kullanici_adi kullanici_adi
passwd kullanici_adi
#### Ev dizinini şifreleme Dizini açma  
* şifreli dizin üzerinde çalışma  
cryptsetup luksOpen /dev/sda2 map1  
/dev/mapper/map1  
mount  /dev/mapper/map1 /mnt  
cryptsetup luksOpen /dev/sda2 map1  
  
Kullanıcıya giriş yapılacak açık anahtar yüklenir.  
ssh-copy-id -i açık_anahtar kullanıcı_adı@IP_adresi  
  
SSH servisi yapılandırma dosyasına göre çalışması için yeniden başlatılır.   
/etc/init.d/ssh restart  
cp /etc/ssh/sshd_config{,.bck}
  
#### Kullanıcı Ayarları:  
su -l orkung
mkdir -p ~/Git_Repolari/kisisel/public
cd ~/Git_Repolari/kisisel/public
git clone https://github.com/ogny/nixfiles.git
cd system_configuration  
rsync -avh --exclude .git --exclude X11 --exclude etc --exclude .gitconfig ./ ~/  
  
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
1) http://emrah.com/notlar/debian_kurulum_notlari_jessie.txt  
2) http://www.serdaraytekin.com/docs/os/debian/debian-apt-pinning.html  
3) http://www.serdaraytekin.com/docs/os/debian/debian-cron-update.html  
4) http://emrah.com/notlar/ssh_notlari.txt  
5) http://emrah.com/notlar/vim_notlari.txt  
6) http://www.serdaraytekin.com/docs/os/debian/sss/  
  
 pip install cheat
`gem install ncurses-ruby`
