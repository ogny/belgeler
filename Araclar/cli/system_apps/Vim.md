* register

kopyala-yapistir vim'de aslinda default register'daki degeri alip verme gibi.
bundan baska istedigin kadar register atayabiliyorsun, default olan
disindakileri cagirmak icin; `"ayy` `"ap` gibi a'ya atadigini kopyala-yapistir
yapabilirsin.

* buyuk kucuk harf donusturme;
Degistirecegin yeri secip visual mode'a gec
`U Uppercase u Lowercase`

* satir atlama: jump --> j satir sayisi
`5j`	

* imlecin bir altindaki satirla bulundugu satiri birlestirme; 
`J`	

* normal modda `e` kelimenin son karakterine goturur. 
* Pencere acikken buffer'i kapatma
`:Bclose`

* Tum satiri secme 
`shift+v` 

* pencereleri yeniden boyutlandirma 
```
c+w sayi +-
:res +-sayi
```

* bos buffer'da bir dosya acma 
:n
* buffer'lar arasi gezinme;
:bn, :bp

* bos satirlari silme;
:%s/^[^a-zA-Z0-9]$\n//ig
:g/^$/d

* her satir sonuna 2 bos karakter ekle 
:%norm A  

* her satirin sonundaki boslugu sil
`:%s/\s\+$//`

 %       = for every line
 norm    = type the following commands
 A*      = append '*' to the end of current line

* bosluklardan kesip yeni sutunlar yapma;
`:%s/ /\r/g`

* Bir kisayola iki kisayol ait komut atama; kisayol tuslari arasina \| eklenir.
`:nnoremap f }) \| zz`

* mark yonetimi
`mx` ile satiri mark'la
`'x` ile bookmark'ladigin satira geri git.
buradaki `x` degerini kucuk harf kullanirsan lokal mark kullanirsin, buyuk
harf kullanirsan, farkli bir dosyada da olsan, cagirdiginda mark'ladigin satiri acar.

* bookmark yonetimi (kendi tus duzenime gore)
  - imlecin bulundugu satiri isaretleme; `\m`
  - isarete not ekleme ; `\i`
  - isaretler arasinda gezinmek; `\n`
  - tum isaretleri bir tamponda gorme; `\a`
  - bulundugun tampondaki isaretleri silme; `\c`
  - tum isaretleri silme `\x`


* Vimdiff'teyken gorunumu dikey-yatay olarak degistirmek;
<C-w>J (or <C-w>K) when you are in vertical split and <C-w>H (<C-w>L) in horizontal

* Metin icerisindeki turkce karakterleri aratmak;
:set digraph
Ctrl-k c, -> ç
Ctrl-k C, -> Ç
Ctrl-k g( -> ğ
Ctrl-k G> -> Ğ
Ctrl-k i. -> ı
Ctrl-k I. -> I
Ctrl-k s, -> ş
Ctrl-k S, -> Ş
Ctrl-k u: -> ü
Ctrl-k U: -> Ü


* Buffer'larla calisma
Bu alanda sik kullanilan cesitli eklentiler mevcut, bunlardan Unite.vim ile
calisacagim.
`:Unite-vertical file buffer` ile nerdtree'deki gibi dosyalari ve buffer'lari goruntuleyebiliyoruz.


* satirlari sutuna cevirme; donusturulecek satira gecip,
`set relativenumber` ile kac satir oldugunu ogren
sayiJ ile birlestir.


* tab duzenlemeleri;
* vertical help icin :vertical yaz.

* Aramada  regex pattern'lerini kullanma;
\<aranacak_pattern\>

* satir sonlarindaki `^M` `^L`  karakterleri silme.
```
%s/\r/\r/g
:%s/^L//g
```


* json biceminde olup da duz metin gibi gorunen tamponu okunakli hale getirmek
icin;
```
:%!python -m json.tool
```
* .vimrc'yi cagirmadan baslatma
```
vim -u NORC
```

* .vimrc'yi guncelleme (source etme)
```
:so $VIMRC
```

* SignColumn'u gizleme
```
sign unplace *
```

* find'la birden cok dosyayi bulup vim'le acmak icin en pratik yol
```
vim $(find . -type f -iname "*.uzanti")
vim $(find . -type f)
```

#### Vim help'ten notlar:
* sadece help penceresini acma;
help |only

* belli bir basliga direk gitme;
imleci basliga gotur ve CTRL-]
geri gelmek icin CTRL-T veya CTRL-O (tekrar bastiginda daha eskiye gider)

* help'te arama yapmak
:helpgrep ile kelime ara (ignorecase icin \G)
aranan kelimenin gectigi yerleri gormek icin :cwindow


* :global komutu;
global commands work by first scanning through the [range] lines and
marking each line where a match occurs (for a multi-line pattern, only the
start of the match matters).

* fold'lama, acma, toggle; `zc``zo` `za`

* Regex karakterleri
\r break line
\s white space

* her dolu satirin altina bos bir satir ekle;
```
:%s/.*\n.*\n.*\n/\0\r/g
```
* her satir sonuna bir bos satir ekle
```
:%s/\(.*\n\)\{1\}/&\
```
* tablo olusturma


```
http://vim.wikia.com/wiki/Working_with_CSV_files
```

* sayfadaki yerel link'i yeni buffer'da acma: `gf`
* sayfadaki url'i tarayicida acma: `gx`


### eklentiler 
* Kisa Kisa kullandigim eklentileri tanitmak istiyorum.

bclose.vim
goyo.vim
L9
neocomplete.vim
neomru.vim
neosnippet.vim
nginx.vim
nerdtree
vim-easymotion
vim-json
vimroom
vim-surround
vim-tmux-navigator
vim-zenroom2
vim-buffergator


#### Eklenti Notlari;
* unite vimproc ve neobundle'dan henuz verim alamadigimdan listeden cikarttim.

* nerdtree
  - window split orientation'u var, sirayla aciyorsun, once horizontal, sonra vertical calisiyorsun
  - <shift-i> ile gizli dosyalari toggle et.    

* easymotion
backward search: <header><header>b

Ultisnips
---

snippet'i devreye almak icin, tab'a basip snippet kisayolunu secip shit+tab
yap.

Python Mode
---

* herhangi bir komutun uzerinde K 'ya bastiginda dokumanini acar.

* <leader> r ile calistirip ciktisini yeni buffer'a doker
```
Neosnippet ile python snippet'lerini kullanma
```
* buffergator `<leader> b` ile left sidebar'da listeliyorsun

vim-bookmarks
---

* 0 basarili, 1 basarisiz


* sutun gizleme (gizlenen bolumu satira fold yaparak)
```
:syn match Concealed '^.\{satir_sayisi\}' conceal
:set conceallevel=2
```

* vim buffer list `:ls`
* case senstive'i kaldirmak icin
`:set ignorecase`

#### Tanimlar: 

the buffer is your file proxy,
the window is a view into a buffer,
the tab page is a "workspace", an arrangement of windows.
Forget all your old habits as they will slow your learning down.


* window-buffer yonetimi
  - bdelete: buffer'i tum pencerelerde kapatir. tum pencerelerde ayni buffer
    aciksa, buffer'i oldurmek icin tum pencereleri de kapatir.

#### Git ile calisma
* merginal ve fugitive kurulur
* dosyada duzenleme bittiginde kaydedilir.
* `:Gstatus` ve `-` ile `git add` 
* `:Gcommit` 
* `:Merginal` ile branch secilir
* `ps` ile push edilir
* asil branch'e gecilip `mn` ile  merge et (test edilmedi.)


##### branch'lerin merge edilmesi disinda, merginal'e gerek yok, process soyle:
* dosyada duzenleme bittiginde kaydedilir.
* `L+g` ve `-` `cc` buffer'i kaydet kapat `L+p` ile gonder


