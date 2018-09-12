### Aptitude
sudo aptitude hold paket_adı veya sudo apt-mark hold paket_adı 
Sistemde askıya alınmış paket olup olmadığını kontrol etmek için;
sudo aptitude search "~ahold"

* kurulu paketleri listeleme
```
zgrep " install " /var/log/dpkg.log.8.gz | awk '{print$4}' |cut -d ':' -f -1 |less
```


#### paketle ilgili bilgi
* repo'lardaki paketle ilgili kisa bilgi. (kac repo'da varsa hepsini
  gorebiliyorsun)
```
apt-cache policy <paket_adi>
```
* genel bilgi
```
apt list --upgradable
```
* paket hakkinda bilgi, detayli bilgi
```
apt-cache show <paket_adi>
apt-cache showpkg <paket_adi>
```

* apt-listchanges — Show new changelog entries from Debian package archives 

# Apt-get
--no-install-recommends
Do not consider recommended packages as a dependency for installing.
Configuration Item: APT::Install-Recommends.

--install-suggests
Consider suggested packages as a dependency for installing.
Configuration Item: APT::Install-Suggests. 

### dotdeb.org repository'sinden paket kurmadan once;
```
gpg --keyserver keys.gnupg.net --recv-key 89DF5277
gpg -a --export 89DF5277 | apt-key add - 
```
* Yuklenen paketleri tarih sirasina gore listeleme;
```
grep install /var/log/dpkg.log
```
* Paket kaldirma: `dpkg -r` ile  kaldiramadiginda purge (-P) parametresini kullan.

* spesifik repo'dan paket kurma;
```
aptitude install -t <repo_adi> <paket_adi>
```

* repo adindan emin degilsin ama o repodaki paketi kurmak istiyorsan, apt-cache
  showpkg ile paket surumunu al,
```
apt-cache showpkg <paket_adi>
apt-get install <paket_adi>=<surum>
```

* sistemde sadece belli bir paketi update etme:
https://help.ubuntu.com/community/PinningHowto
https://help.ubuntu.com/community/UbuntuBackports

* herhangi bir guvenilir anahtlari kaldirma;
```
apt-key del <key_id>
```
