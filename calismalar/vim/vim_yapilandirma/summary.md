
~~daha once kendi kendime buldugum yeni tab acip fzf'yle aramayi replace
edebilecek duzeyde gelismis degil, ancak bazen faydali olacagina eminim.
ornegin farkli bir tmux session'undayken kullanmak gibi.

kullanim aliskanligina ekleyebilecegin bir ozellik, 
template'i server olarak baslat, yeni acilacak buffer'lari terminal'den ona
gonder (not: imlec hangi penceredeyse buffer'i orada aciyor);

```bash
vim --servername test -S template.vim
vim --servername test --remote-silent abidik.md
```
