* [Pubkeyim](http://pgp.mit.edu/pks/lookup?op=get&search=0x3EEFC17F17D9BD2F)
* fingerprint: 6D4C C8AC D60B 1E9F 5308  54C6 3EEF C17F 17D9 BD2F 

* [Cheatsheet](irtfweb.ifa.hawaii.edu/~lockhart/gpg/gpg-cs.html)  
* list the keys in your public key ring:  
gpg --list-keys  
  
* list the keys in your secret key ring:  
gpg --list-secret-keys  
  
### agent ile gpg her cagrildiginda parola girmemek icin;  
* ~/.gnupg/gpg.conf'a eklenecek;  
** use-agent  
* ~/.zshrc'ye eklenecek;  
** export GPGKEY=<Buraya key id'niz gelecek>  
** export GPG_TTY=$(tty)   
* gpg-agent'ta herhangi bir sorun olustugunda PID'den oldurup yeni bir terminal
actiginda sorun kalkiyor
