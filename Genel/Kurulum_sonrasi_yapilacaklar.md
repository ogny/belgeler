### Genel
#### Oncelikle yapilacaklar
```
apt install -y --force-yes sudo
sudo apt purge nano
```
* user'a passwordless sudo yap `visudo` (kurulumdan sonra kaldirilacak)
`orkung ALL=(ALL) NOPASSWD: ALL`
* root ve user'in default login shell'ini zsh yap. Kullaniciya gec.
* kulllanicinin cache dizinini tmpfs yap
* recommended ve suggested paketleri kurma
```
echo 'APT::Install-Recommends "0";
APT::Install-Suggests "0";' | sudo tee /etc/apt/apt.conf.d/80recommends
sudo apt install zsh tmux vim-gtk git apt-transport-https
rm -rf ~/.cache
mkdir  ~/.cache
sudo mount -t tmpfs -o  size=1G,nr_inodes=10k,noatime,mode=777,uid=orkung,gid=orkung tmpfs ~/.cache
```
* /etc/fstab'a noatime eklenecek.
```
sudo vim /etc/fstab
# cache tmfps 
tmpfs           /home/orkung/.cache        tmpfs            size=1G,nr_inodes=10k,noatime,mode=777,uid=orkung,gid=orkung        0  0
sudo vim /etc/passwd
sudo vim /etc/modprobe.d/blacklist.conf
# speaker modulu iptal
blacklist pcspkr
sudo vim /etc/dhcp/dhclient.conf
prepend domain-name-servers 208.67.220.220, 8.8.4.4;
timeout 10;
su - orkung
mkdir -p ~/Git_Repolari/{kisisel,diger,is}
mkdir -p ~/Git_Repolari/kisisel/{public,diger}
git clone https://github.com/ogny/nixfiles.git ~/Git_Repolari/kisisel/public/
git clone https://github.com/ogny/belgeler.git ~/Git_Repolari/kisisel/public/
git clone https://github.com/ogny/ogny.github.io.git ~/Git_Repolari/kisisel/public/
git clone git@bitbucket.org:ogny/server-side.git ~/Git_Repolari/kisisel/private/
curl -kL https://raw.github.com/cstrap/monaco-font/master/install-font-ubuntu.sh | bash
wget -O- https://code.bitlbee.org/debian/release.key | sudo apt-key add -
wget -O- https://jgeboski.github.io/obs.key | sudo apt-key add -
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
sudo apt -y --force-yes install deb-multimedia-keyring
ln -s ~/Git_Repolari/kisisel/public/nixfiles/dotfiles/* ~/
ln -s ~/Git_Repolari/kisisel/public/nixfiles/.config/VirtualBox.xml ~/.config/VirtualBox/
ln -s ~/Git_Repolari/kisisel/private/server-side/conf/user/* ~/
ln -s ~/Git_Repolari/kisisel/public/nixfiles/Xfiles/* ~/
ln -s ~/Git_Repolari/kisisel/public/nixfiles/bin ~/
ln -s ~/Git_Repolari/kisisel/private/server-side/conf/user/.pip ~/
sudo ln -s ~/Git_Repolari/kisisel/private/server-side/conf/system/hosts /etc/
sudo ln -s ~/Git_Repolari/kisisel/private/server-side/conf/system/vpnc_default.conf \
/etc/vpnc/default.conf
sudo ln -s ~/Git_Repolari/kisisel/private/server-side/conf/system/autofs/* /etc/
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
deb http://www.deb-multimedia.org jessie main contrib non-free
deb-src http://www.deb-multimedia.org jessie main contrib non-free
```
* Uygulama Repo'lari
```
echo "deb http://download.opensuse.org/repositories/home:/jgeboski/jessie ./" \
| sudo tee /etc/apt/sources.list.d/jgeboski.list"
echo "deb http://code.bitlbee.org/debian/master/jessie/amd64 ./" \
| sudo tee /etc/apt/sources.list.d/bitlbee.list
echo "deb http://downloads.hipchat.com/linux/apt stable main" \
| sudo tee /etc/apt/sources.list.d/hipchat.list
echo "deb http://dl.google.com/linux/chrome/deb/ stable main" \
| sudo tee /etc/apt/sources.list.d/google_chrome.list
echo "deb https://apt.dockerproject.org/repo debian-jessie main \
| sudo tee /etc/apt/sources.list.d/docker.list
```
* X kurulumu bittiginde startx ile login ol
```
sudo apt install -y --force-yes --install-recommends xorg
sudo apt install -y --force-yes -t jessie-backports --install-recommends i3  i3-wm-dbg
ln -s ~/Git_Repolari/kisisel/public/nixfiles/Xfiles/.i3_ofis ~/.i3
sudo cat << EOF >>  /usr/share/X11/xorg.conf.d/50-synaptics.conf
Section "InputClass"
  Identifier "touchpad catchall"
  Driver "synaptics"
  MatchIsTouchpad "on"
  MatchDevicePath "/dev/input/event*"

  Option "TapButton1" "1"

EndSection
EOF
```
* Guncelleme
```
sudo apt update && \
sudo apt -dy dist-upgrade && \
sudo apt-get autoclean && \
sudo apt-get autoremove -y && \
sudo apt -y dist-upgrade 
```
* Silinecekler
```
sudo apt purge nfs-common rpcbind installation-report \
reportbug tasksel tasksel-data task-english os-prober
```
* Gerekli paketler
```
\curl -sSL https://get.rvm.io | bash -s stable --ruby
sudo dpkg --add-architecture i386 && sudo apt update
sudo apt install -y --force-yes -t jessie-backports --install-recommends \
owncloud-client sysdig sysdig-dkms
sudo apt install -y --force-yes --install-recommends \
linux-headers-$(uname -r|sed 's,[^-]*-[^-]*-,,')  virtualbox virtualbox-qt 
sudo apt install -y --force-yes --install-recommends openvpn sysdig sysdig-dkms
sudo apt install -y --force-yes -t deb-multimedia mplayer ffmpeg wine wine32
sudo apt install -y --force-yes google-chrome-stable hipchat weechat sqlitebrowser \
weechat-plugins bitlbee bitlbee-facebook weechat-scripts alsa-base alsa-utils \
pulseaudio pulseaudio-utils pavucontrol ttf-dejavu-core ttf-dejavu-extra scrot \
feh redshift unclutter numlockx xtrlock xbindkeys screenfetch i3status wodim \
ack ranger dos2unix jmtpfs pandoc suckless-tools tpp bzip2 zip rpm2cpio \
unzip diffutils patch tree ncdu postgresql-client postgresql postgresql-contrib \ 
git-flow git-extras tig virtualenvwrapper python-pip python-lxml python-dev \
docker-engine dkms acct ca-certificates apt-listchanges  bundler \
dstat coreutils gnupg parted autofs htop dosfstools parallel sysstat ccrypt \
cryptsetup pwgen dbus-x11 wicd wicd-daemon wicd-curses whois cisco-vpnc \
mtr-tiny sshpass sshuttle curl lftp rfkill resolvconf apache2-utils rdesktop \
dnsutils iftop pptp-linux nuttcp
sudo pip install cheat geeknote python-gist pika howdoi
sudo gem install ncurses-ruby 
sudo systemctl start pulseaudio
sudo systemctl start alsa-state.service
mkdir ~/projects ~/.virtualenvs
ln -s ~/Git_Repolari/kisisel/public/nixfiles/other_dotfiles/.virtualenvs/* ~/.virtualenvs/
source /usr/local/bin/virtualenvwrapper.sh
```
* Manuel
  - rxvt-unicode-256color_9.19-1.1_amd64.deb
  - 1a23_setup_sst53.exe (wine ile kurulum)
  - chefdk.tar.bz2 --> /opt altina 
  - lokal_user_chefdk_for_jekyll.tar.bz2
  - diger_git_repolari.tar.bz2
  - chrome eklentisi "modern new tab page"
  - vim.tar.bz2
  - ~/.rvm alti alinacak
  - ~/.config/google-chrome alinacak
```
ln -s ~/Git_Repolari/kisisel/public/nixfiles/other_dotfiles/.vim/UltiSnips ~./vim/
sudo ln -s bin/ps_mem.py /usr/local/bin/
sudo resolvconf --disable-updates
vimsed github repo
```

### Duzenleme
* wicd'te kullandigin baglanti> sag ok >"automatically connect to this network"
* kullanicidan parolasiz root yetkisini kaldir. ornegin `shuttle` icin ver `visudo`
```

```

#### Ev dizinini şifreleme Dizini açma  
* şifreli dizin üzerinde çalışma  
```
cryptsetup luksOpen /dev/sda2 map1 /dev/mapper/map1 
mount /dev/mapper/map1 /mnt 
cryptsetup luksOpen /dev/sda2 map1  
```
  
#### Diger Notlar
```
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
* default kurulumla geliyorlar mi test et `tree, curl, mlocate`
