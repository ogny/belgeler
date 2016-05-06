#### Surum:7
```
yum install -y vim-enhanced tmux epel-release wget
mkdir -p /root/.vim/syntax/
cp /usr/share/doc/tmux-1.8/examples/tmux.vim ~/.vim/syntax/
cp /usr/share/doc/tmux-1.8/examples/screen-keys.conf ~/.tmux.conf
tmux
echo "set -o vi" >> ~/.bashrc
```
