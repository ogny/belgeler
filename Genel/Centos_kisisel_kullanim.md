#### Surum:7
```
ymkdir -p /root/.vim/syntaxum install -y vim-enhanced tmux epel-release wget
mkdir -p /root/.vim/syntax/
cp /usr/share/doc/tmux-1.6/examples/tmux.vim ~/.vim/syntax/
cp /usr/share/doc/tmux-1.6/examples/screen-keys.conf ~/.tmux.conf
tmux
echo "set -o vi" >> ~/.bashrc
```
