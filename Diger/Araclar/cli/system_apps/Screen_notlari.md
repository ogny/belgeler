* window'u bolme - diger window'a gecis yapma - yeni prompt acma;
`
 ^A S
 ^A Tab
 ^A C
`
* boluneni geri kapama
C-a X

* geriye dogru arama (tmux benzer)
```
^a esc
^a [
```

* tmux icinde screen'de clear'i calistirmak icin;
`set -o vi`

* gecerli screenrc'yi atlayarak screen'i default ayarlariyla baslatmak;
```
screen -c /etc/screenrc
```

* screen oturumuna yeniden baglandiginda, pencere boyutu, ciktigindaki gibi, kucuk kalirsa

```
ctrl-a F 
screen -x -A
```
* session'u ad vererek olusturma
```
screen -S mySessionName
```
