
/set weechat.bar.status.color_bg black
/set weechat.bar.status.items off
/set weechat.bar.title.hidden on
/set weechat.look.buffer_time_format ""      
/set plugins.var.perl.highmon.output bar
/set plugins.var.perl.highmon.bar_lines 250
/alias clear_highmon /mute /set plugins.var.perl.highmon.bar_lines -1;/mute /set weechat.bar.highmon.items "";/mute /set weechat.bar.highmon.items "highmon";/mute /set plugins.var.perl.highmon.bar_lines 250
/set irc.look.smart_filter on
/filter add jpk * irc_join,irc_part,irc_quit *
/bar toggle nicklist
 /bar set nicklist  size 35
 /toggle_nicklist add
in channel
/set plugins.var.python.toggle_nicklist.action "show"
now all nicklists will be hidden except the channels you add
server degistirme ^x

* Bir kanala otomatik giris yapmak;
/set irc.server.freenode.autojoin ""
http://www.kodama.gr/2011/08/19/weechat-horizontal-buffers-bar/
*̶ ̶h̶t̶t̶p̶:̶/̶/̶p̶a̶s̶c̶a̶l̶p̶o̶i̶t̶r̶a̶s̶.̶c̶o̶m̶/̶m̶y̶-̶w̶e̶e̶c̶h̶a̶t̶-̶c̶o̶n̶f̶i̶g̶u̶r̶a̶t̶i̶o̶n̶/̶ ̶

### Loglama seviyeleri
1 user message, notice, private
2 nick change
3 server message
4 join/part/quit
9 all other messages

##### sunucu adi/kanal adina gore log dosyasi olusturmak icin;
/set logger.file.mask '$server/$channel/%Y-%m.log'

##### kanal bazli log'lama;
* freenode buffer'ini loglama
/set logger.level.irc.server.freenode 9
* kanali loglama
/set logger.level.irc.freenode.#weechat 3
* tum kanallari loglama
/set logger.level.irc.freenode 3 
