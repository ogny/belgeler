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
