yontem; hicbir sey silinmeyecek, isi biten note "tamamlanan" notebook'una tasinacak. 

### Alias'lar
bash/zsh profile'a eklenecek;
```
alias gecr='geeknote create --title $1'
alias geed='geeknote edit --note $1'
alias gefs='geeknote find --notebooks "kisisel" --search $1'
```

### Komutlar
* `geeknote tag-list`

* yeni note olustururken tag'lama:
```
gecr <not_adi> --tags "<tag_1>,<tag_2>"
```

* varolan note'in taglarini degistirme
```
geed <not_adi> --tags "<tag_1>,<tag_2>"
```

* note'u spesifik notebook'ta, tag'la olusturma 
```
gecr <not_adi> --notebook <notebook_adi> --tags "<tag_1>"
```

* note'u tamamlanan notebook'una tasima
```
geed <not_adi> --notebook "tamamlanan"
```

### Arama
* tag'a gore
```
geeknote find --search "<text_to_find>" --tags "<tag_1>,<tag_2>"
```
* find by default title'a gore arama yapiyor, --content-search ile arama
  yapilacaksa direk `show` kullanmak daha pratik.
```
geeknote show <*text to search and show*>
```

### Misc

Use ` :GeeknoteSaveAsNote  `
acik sayfanin ilk satirini baslik olarak kaydediyor
* To move a note into a different notebook, simply move the note's text
(includes title and GUID) under the desired notebook in the navigation window
and save the buffer. Similar to renaming, any number notes can be moved
before saving the buffer.

[Kaynak](https://github.com/neilagabriel/vim-geeknote)

* Geeknote strikethrough'un markdown'unu desteklemiyor, direk html tag'ini
  yazmak gerek; `<s></s>`


### Notebook adlarim

* akilfikir
* bet
* gunluk
* is
* kitapnotlari
* kisisel
* mucizeyim
* tamamlanan
* yenialiskanlik

#. Not kaydetme; acik sayfanin ilk satirini baslik olarak kaydediyor::

    GeeknoteSaveAsNote

#. Notu bulundugu notebook'tan farkli birine tasimak icin yank/paste yeterli.
   (rename de ayni)

#. notlari guncellemek icin buffer'lar arasinda gezinip `:wa` yapman yeterli 

[Kaynak](https://github.com/neilagabriel/vim-geeknote)
