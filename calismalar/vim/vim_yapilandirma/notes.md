#### Server-client model

bir ana pencerenin acilip, diger acilacak buffer'larin komut satirindan o ana
pencereden acilmasini saglama, ornek;
```bash
vim --servername foo
vim --servername foo --remote-silent bar.txt
```
kolay calismak icin gerekli zsh'ta bazi ayarlar yaptim;
* eset = VI_SERVER degiskenine atama yapiyor
* vs = VI_SERVER degiskeniyle template'i aciyor.
* acilacak yeni shell'ler de degiskeni alsin diye vim --serverlist ciktisitini
  VI_SERVER'a export ediyorum.
* es ile acilacak dosyalari yeni buffer'la server'a gonderiyorum

Calismanin ana kaynagi:
[A PRODUCTIVE WORKFLOW WITH VIM SESSIONS AND SERVERS](https://www.rohanjain.in/yet-another-vim-productivity-post-server-client/)
