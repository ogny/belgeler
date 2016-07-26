* pls dosyasi calmak icin: -playlist
* cache problemi yasamamak icin: -lavdopts threads=2
* Ses yoneticisi olarak pulse-alsa'yi kullanmasi icin `~/.mplayer/config`'e ekle;
```
ao=pulse,alsa,sdl:aalib
```
https://bugs.launchpad.net/ubuntu/+source/mplayer/+bug/550517

* repeat icin -loop <repeat count> sonsuz icin 0

* son yuklenenleri calma
```
find . -type f -iname "*.mp3" -mtime -1 -exec mplayer -loop 0 {} \+ 
```
