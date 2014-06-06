--existing ile sadece varolan dosyalari guncelliyoruz;
rsync -avzuhe "ssh -l kullanici" IP_ADRESI:/etc/nginx/ etc/nginx/ --existing

--exclude ile hedeften guncellenmesini istemedigimiz klasorleri exclude edebiliyoruz;
rsync -avzuhe "ssh -l kullanici" IP_ADRESI:/etc/nginx/ etc/nginx/ --exclude='sites-enabled'

--
