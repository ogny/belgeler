--existing ile sadece varolan dosyalari guncelliyoruz;
rsync -avzuhe "ssh -l kullanici" IP_ADRESI:/etc/nginx/ etc/nginx/ --existing

--exclude ile hedeften guncellenmesini istemedigimiz klasorleri exclude edebiliyoruz;
rsync -avzuhe "ssh -l kullanici" IP_ADRESI:/etc/nginx/ etc/nginx/ --exclude='sites-enabled'

* yedek almak icin;
 -a --verbose --recursive --links --perms --executability --owner --group
 --time

* linklenmis dosyalarin gonderilmesi
ssh ile uzak sunucuya rsync'le dosya gonderilirken -l secenegi kullanilamiyor,
hata veriyor.

* rsync'le kaynaktaki dosyalari hedefe tasimak icin; (kopyalamadan) 
```
rsync  --remove-source-files --compress-level=1 \
-av  kaynak_makina:/*.dosya_uzantisi hedef_makina:/dizin
```
