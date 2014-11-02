* pdf'e cevirmek icin kurulmasi gereken paketler;
texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended

* txt'den epub'a donusturme; txt'de read mode girmiyoruz.
pandoc -w epub -o haproxy_configuration.epub haproxy_configuration.txt 

* markdown'dan donusturme
pandoc -r markdown -w epub --epub-metadata=*metadatafile*.xml
--epub-cover-image=*coverimage*.jpg --epub-stylesheet=*stylesheet*.css -o
*yourfilename*.epub *yourfilename*.md

markdown'dan reStructuredText'e donusturme
pandoc -s <dosya.md> -t rst -o <dosya.rst>
