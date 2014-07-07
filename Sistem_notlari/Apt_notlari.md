### Aptitude
sudo aptitude hold paket_adı veya sudo apt-mark hold paket_adı 
Sistemde askıya alınmış paket olup olmadığını kontrol etmek için;
sudo aptitude search "~ahold"

paketlerin surumlerini repo'dakilerle kurulu olanlar arasindak kiyaslama;
apt-cache policy 

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
