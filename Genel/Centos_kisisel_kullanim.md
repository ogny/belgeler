```
yum install -y vim-enhanced epel-release wget
yum install -y tmux 
mkdir -p /root/.vim/syntax/
cp /usr/share/doc/tmux-1.6/examples/tmux.vim ~/.vim/syntax/
cp /usr/share/doc/tmux-1.6/examples/screen-keys.conf ~/.tmux.conf
vim ~/.tmux.conf
:75 ve sonrasi comment
unbind [
bind Escape copy-mode
tmux
echo "set -o vi" >> ~/.bashrc
#```
unbind [ 
bind Escape copy-mode  ]
