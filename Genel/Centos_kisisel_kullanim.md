```
yum install -y vim-enhanced epel-release wget git
yum install -y tmux 
mkdir -p /root/.vim/syntax/
cp /usr/share/doc/tmux-1.8/examples/tmux.vim ~/.vim/syntax/
cp /usr/share/doc/tmux-1.8/examples/screen-keys.conf ~/.tmux.conf
vim ~/.tmux.conf
:75 ve sonrasi comment
unbind [
bind Escape copy-mode
tmux new-session -s orkun
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
vim ~/.vimrc
if !has("compatible")
    call plug#begin('~/.vim/plugged')
    Plug 'scrooloose/nerdtree'
    call plug#end()
endif
filetype plugin on
filetype plugin indent on
filetype indent on
set nocompatible                " be iMproved, required
filetype on
syntax enable
nnoremap  <C-n> :bn<cr>
nnoremap  <C-p> :bp<cr>
set tabstop=4
```
