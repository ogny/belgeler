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
