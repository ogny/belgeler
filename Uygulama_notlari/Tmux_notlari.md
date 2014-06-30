:rename-session -t eski yeni
:kill-session -t ad

* Reloading tmux config
:source-file ~/.tmux.conf

* new-session -s session_name

* pencerenin %25'i kadar dikey yeni bir window acip vim baslatma
split-window -h -p 25 vim
