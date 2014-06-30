* mp4-mp3 donusturme;
ffmpeg -i k.mp4 -q:a 0 -map a k.mp3
	* mp4-mp3 belli bir araligi donusturme;
ffmpeg -ss 00:00 -i k.mp4 -q:a 0  -map a -t 00:54 k.mp3

* ekranda oynayan görüntüyü kaydetme
ffmpeg -f x11grab -r 25 -s 1024x768 -i :0.0 /tmp/outputFile.mpg

* herhangi bir formattaki yayini alip oynatma
ffmpeg -i sample.mp4 -vcodec copy -acodec copy output.mp4
(use 'copy' to bypass audio encoding)

ffmpeg'le vidyonun icerigine bakildiginda, son kisminda hakkinda bilgi veriyor;
 Metadata:
    major_brand     : mp41
    minor_version   : 0
    compatible_brands: qt  iso2
    creation_time   : 2013-11-11 11:17:49
  Duration: 00:00:04.04, start: 0.000000, bitrate: 7480 kb/s
    Stream #0:0(eng): Audio: aac (mp4a / 0x6134706D), 48000 Hz,       stereo, s16, 128 kb/s
    Metadata:
      creation_time   : 2013-11-11 11:17:49
    Stream #0:1(eng): Video: h264 (High) (avc1 / 0x31637661),         yuv420p, 720x1280, 7346 kb/s, 25.25 fps, 25 tbr, 48k tbn, 96k tbc

