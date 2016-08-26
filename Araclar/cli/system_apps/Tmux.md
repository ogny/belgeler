#. Oturumu yeniden adlandirma::
```
:rename-session -t eski yeni
<META>$
```
#. oturumu öldürme::
```
:kill-session -t ad
```
#. Reloading tmux config::
```
:source-file ~/.tmux.conf
```
#. Yeni oturum acma::
```
new-session -s session_name
```
#. pencerenin %25'i kadar dikey yeni bir pencere açıp vim başlatma::
```
split-window -h -p 25 vim
```
#. oturumlar arasinda pane gonderme::
```
hedef oturuma gec
join-pane -s kaynak_oturum:pane_id'si
```
`kaynak <https://forums.pragprog.com/forums/242/topics/10533>`

#. pencereleri yeniden siralama::
```
move-window ile var olmayan bir pencereye atayabiliyoruz. Ornek:
`move-window -s 1:6 -t 0:6`
swap-window ile varolanlarin siralamasini değiştirebiliyoruz.
swap-window -s 3 -t 1
```

`kaynak <http://superuser.com/questions/343572/how-do-i-reorder-tmux-windows>`

#. iki haneli pencerelere gecme; index modunda gececegimiz pencereyi girebiliriz::
```
prefix+ '
```
`kaynak <http://stackoverflow.com/questions/25335730/how-do-i-jump-to-double-digit-window-number-in-tmux>`_

#. copy mode'u sectigimizde olusan gecikmeyi engelleme::
```
set -sg escape-time 0
```
#. ayni window'da acik pane'lerde ayni komutu calistirma::
```
:setw synchronize-panes 
```
#. Pane'i ayni session'daki baska bir window'a gonderme
kaynak pane'e gec, hedef pane'in yolunu goster::  
  ```
C-b :move-pane -t :3.2 
```

Update Environment
----

* Set a space-separated string containing a list of environment variables to
be copied into the session environment when a new session is created or an
existing session is attached.  Any variables that do not exist in the
source environment are set to be removed from the session environment (as
if -r was given to the set-environment command).  The default is "DISPLAY
SSH_ASKPASS SSH_AUTH_SOCK SSH_AGENT_PID SSH_CONNECTION WINDOWID XAUTHOR‐
ITY".

Eklentiler
----------

`tmux-copycat <https://github.com/tmux-plugins/tmux-copycat>`_

Search
```
prefix + / - regex search (strings work too)
```
Example search entries:

foo - searches for string foo
[0-9]+ - regex search for numbers
Grep is used for searching.
Searches are case insensitive.

Predefined searches

prefix + ctrl-f - simple #.f#.ile search
prefix + ctrl-g - jumping over #.g#.it status files (best used after git status command)
prefix + ctrl-u - #.u#.rl search (http, ftp and git urls)
prefix + ctrl-d - number search (mnemonic d, as digit)
prefix + alt-i - #.i#.p address search
These start "copycat mode" and jump to first match.

"Copycat mode" bindings

These are enabled when you search with copycat:

n - jumps to the next match
N - jumps to the previous match

* oturumu yeniden adlandirma::
```
Ctrl + B, $
```

```
* Kullanim aliskanliklarina ekle; pane'i yeni session'a cevirme::
```
  prefix + @ 
```
