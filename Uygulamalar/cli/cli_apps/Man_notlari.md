export MANPAGER="/bin/sh -c \"col -b | vim -c 'set ft=man ts=8 nomod nolist nonu noma' -\"" 
source et
set modifiable
:g/^$/d
 
Öncelikle name sekmesi altında programın adı ve kabaca ne işe yaradığı
yazar.

NAME
chown -- change file owner and group
buradan programın ne iş yaptığını öğrenmiş oluyoruz.

Gelelim kullanımına.Nasıl kullanılacağı ise Synopsis kısmında yazar.Buna
geçmeden önce kabaca "regular expression"ını açıklayayım.Örnek olarak ls
programını alalım.

1) Düzyazı: Aynen kabul edilir.
Örneğin : Synopsis i ls -alh şeklindeyse sadece ve sadece ls -alh yazarak o
programı çalıştırabilirsiniz.ls, ls -a , ls-h yazarsanız hata verir

2)[] seçim kutusudur.İçinde barındırdığı 1 veya birkaç parametrenin
kullanılabileceğini veya istenirse hiçbirinin kullanılmayabileceğini
belirtir.

Örneğin: ls -[alh] için ls şeklinde kullanamazsınız. ls -, ls -a, ls -l, ls
-h, ls -al, ls -ah, ls -lh, ls -alh şeklinde kullanabilirsiniz.

3) | or seçimidir.Sağındaki ve solundaki parametreden yalnızca biri
kullanılabilir demektir.

Örneğin ls -[a | L | h] için: ls -, ls -a, ls -L veya ls -h şeklinde
kullanabilirsiniz. ls, ls -al, ls -ah, ls -lh, ls -alh şeklinde
kullanamazsınız.

4) italik yazılar değişken girdileri gösterir
Örneğin ls dizin_adı için ls dizin_adı yazarsanız hiçbirsonuç alamazsınız
muhtemelen.Ama ls /var yazarsanız /var dizinini listeler.

En son örnek olarak: ls [-a | L | h] dizin_adı için bu programın kullanımı:
ls /var
ls -a /var
ls -L /var
ls -h /var olacak.Alttakiler hata verir.Ayrıca sadece ls veya ls -a vs
şekillerde de kullanamazsınız

5) her bir parametrenin ne iş yaptığını manual'in devamında aşağıda yazar.
Şimdi gelelim chown programının synopsisine.

chown [-fhv] [-R [-H | -L | -P]] owner[:group] file ...
chown [-fhv] [-R [-H | -L | -P]] :group file ...

bu ikisinden sadece biri kullanılabilir demek. Biz üsttekini inceleyelim:
chown [-fhv] [-R [-H | -L | -P]] owner[:group] file
bu chown' dan sonra fvh parametrelerini alabilir demek.Veya almayabilir de.

Sonrasında   [-R [-H | -L | -P]] bu kısımda Sadece -R;  Sadece -H,-L veya
-P; veya -RH,-RL,-RP alabilir veya hiçbirini almayabilir demek.

Sonrasında  owner[:group] owner kısmı mutlaka girilecek.Yukarıda anlattığım
1. kurala giriyor. group kısmı ise istenirse girilebilir.Yukarıda
analattığım ikinici kurala giriyor.

Burada owner dosyaya atanıcak yeni kullanıcının adını, group ise dosyaya
atanacak yeni grubu belirtiyor.Zira eski grup, eski kullanıcı, veya
herhangi başka bir kullanıcıdan sözedilmesi mümkün değil.Bizim hedefimiz
dosyayaya veya dizine yeni kullanıcı atamak.

Sonrasında  file kısmı var.Buarda bahsedilen file parametresinin ise
izinleri değiştirilmek istenen dosya veya dizinden farklı birşey olması pek
mümkün değil.Zira böyle bir durumda manual'in devamında mutlaka
belirtilirdi.Bu kısım mutlaka girilmek zorunda.
