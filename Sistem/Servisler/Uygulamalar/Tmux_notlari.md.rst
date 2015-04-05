* Oturumu yeniden adlandirma
```
:rename-session -t eski yeni
```

* oturumu öldürme
```
:kill-session -t ad
```

* Reloading tmux config
```
:source-file ~/.tmux.conf
```

* Yeni oturum acma
```
new-session -s session_name
```

* pencerenin %25'i kadar dikey yeni bir pencere açıp vim başlatma
```
split-window -h -p 25 vim
```

* oturumlar arasinda pane gonderme;
    * hedef oturuma gec
    * join-pane -s kaynak_oturum:pane_id'si
```
[kaynak](https://forums.pragprog.com/forums/242/topics/10533)
```

* pencereleri yeniden siralama;
move-window ile var olmayan bir pencereye atayabiliyoruz.
swap-window ile varolanlarin siralamasini değiştirebiliyoruz.
```
swap-window -s 3 -t 1
```
[kaynak](http://superuser.com/questions/343572/how-do-i-reorder-tmux-windows)

* iki haneli pencerelere gecme; index modunda gececegimiz pencereyi
girebiliriz;
prefix+ '
[kaynak](http://stackoverflow.com/questions/25335730/how-do-i-jump-to-double-digit-window-number-in-tmux)

* copy mode'u sectigimizde olusan gecikmeyi engelleme
```
set -sg escape-time 0
```
#### Eklentiler;
[tmux-copycat](https://github.com/tmux-plugins/tmux-copycat)

Search
prefix + / - regex search (strings work too)
Example search entries:

foo - searches for string foo
[0-9]+ - regex search for numbers
Grep is used for searching.
Searches are case insensitive.

Predefined searches

prefix + ctrl-f - simple *f*ile search
prefix + ctrl-g - jumping over *g*it status files (best used after git status command)
prefix + ctrl-u - *u*rl search (http, ftp and git urls)
prefix + ctrl-d - number search (mnemonic d, as digit)
prefix + alt-i - *i*p address search
These start "copycat mode" and jump to first match.

"Copycat mode" bindings

These are enabled when you search with copycat:

n - jumps to the next match
N - jumps to the previous match


