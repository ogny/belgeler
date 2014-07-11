* Oturumu yeniden adlandirma
```
:rename-session -t eski yeni
```

* oturumu oldurme
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

* pencerenin %25'i kadar dikey yeni bir window acip vim baslatma
```
split-window -h -p 25 vim
```

* Pane'i baska bir tmux oturumuna gonderme;

