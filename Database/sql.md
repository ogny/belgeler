### Kaynaklar

* [Megep](http://www.megep.meb.gov.tr/mte_program_modul/moduller_pdf/Veritaban%C4%B1%20Tasar%C4%B1m%C4%B1.pdf)


### Tanimlar:

* satir=kayit
* sutun=nitelik

#### iliskisel veri tabani
Tablo içerisindeki satırlar tablonun kayıtlarını oluşturur. 
Anahtar alan tablonun tanimlayicisi

##### tablo ozellikleri
* Her satır birbirinden farklı olmalıdır yani birbiri ile tamamen ayni olan iki
  kayıt kullanılmamalıdır.
* Satırların ve sütunların sırasının nasıl olacağı önemli değildir.
* Hücrelerdeki veriler atomik olmalıdır.

Herhangi bir tablodaki her bir satır için kullanılan anahtarın tek olması
gerekmektedir. Aksi takdirde kayıtlar arasında tutarsızlıklar meydana
gelebilmektedir.

Birincil anahtar(primary key) veya yabancı anahtar(foreign key) türlerinden
birisi seçilerek kısıtlamaların gerçekleştirilmesi sağlanmaktadır.

###### Birincil anahtar(primary key)

TC Kimlik No, il plaka no. (06-Ankara,34-İstanbul, 35- İzmir gibi), il telefon
kod no. (312-Ankara,242-Antalya gibi), posta kod no.  bağlı bulundukları
ülkelerde benzersiz numaralardır birincil anahtarları başlarındaki sayısal
ifadelerdir.  

Birincil anahtarlar hiçbir zaman NULL(boş) veya birbiri ile ayni olan değerleri
içeremez.

A primary key is used to uniquely identify each row in a table. It can either
be part of the actual record itself , or it can be an artificial field (one
that has no meaning other than being an identifier of the record). How to set
primary keys is an important factor in database design, as the choices of
primary key can have significant impact on the performance, usability, and the
extensibility of the entire database.  A primary key can consist of one or more
columns on a table. When multiple columns are used as a primary key, they are
called a composite key.  In designing the primary key, we want to make sure we
use as few columns as possible. This is for both storage and performance
reasons. Since primary key information requires additional storage in a
relational database, the more columns a primary key includes, the more storage
it requires. In terms of performance, fewer columns means that the database can
process through the primary key faster since there is less data, and it also
means that table joins will be faster since primary keys, together with foreign
keys, are often used as the joining condition.  A primary key cannot be NULL,
as it does not make sense to use the NULL value to uniquely identify a record.
Therefore, the column that is set as a primary key or as part of a primary key
cannot be NULL.  Primary keys can be specified either when the table is created
(using CREATE TABLE) or by changing the existing table structure (using ALTER
TABLE).  Below are examples for specifying a primary key when creating a table.
Please note when primary key is specified upon table creation, the column(s)
that are set to be the primary key are automatically set to NOT NULL.

##### Yabancı anahtar(Foreign key) 

Tablo içerisindeki verilerin birbirleri ile iletişim kurabilmeleri amacıyla
kullanılan anahtarlardır. Birincil anahtarlar hiçbir zaman NULL(boş) veya
birbiri ile ayni olan değerleri içeremezken, yabancı anahtarlar birbirleri ile
aynı olan değerler içerebilirler. Bir tabloda birden fazla yabancı anahtar
kullanılabilir.  

Yabancı anahtar,bir tabloya girilebilecek verileri başka bir tablonun herhangi
bir alanında yer alabilecek veriler ile sınırlandırmak ve ilişkilendirmek için
kullanılır.  

Yabancı anahtara, başka bir tablonun birincil anahtarıdır da denilebilir.

#### Normalizasyon

##### Veri kisitlamalari

* Ayni tipte bilgiyi farkli sutunlarda tutma
* ayni sutunda birden cok veri tutma
* veriyi gereksiz tekrarlama (cozum foreign key kullanmak)

Cozum amaciyla 1Nf tanimlanmis, ancak bu da update/delete islemlerinde sorun cikariyor, bunun uzerine 2nf tanimlanmis;

1nf'i bolerek olusturuluyor.

* Tablolar bölünürken fonksiyonel bağımlılık göz önünde bulundurulmalıdır.
* Bölünen tablolardan birinin birincil anahtarı ile bölünen diğer tablodaki
  birincil olamayan bir alan arasında bağımlık varsa buna tam bağımlılık denir.
  Bu duruma ikinci normal form adı verilir.

###### 1nf'in 2NF’e dönüştürülmesi

anahtara bağlı olmayan sütunlar anahtara bağlanarak yeni tablolara bölunur

2NF’de,1NF’den farklı olarak tablolar tekrarlı verilerden arındırılmış olup,
anahtar olmayan tüm sütunlar, birincil anahtara tam işlevsel bağımlıdır.

Burada da satir silerken sutunlar arasindaki etkilesimde problemler cikiyor

###### 3nf

Kısmi işlevsel bağımlılıklar ortadan kaldırılarak birinci normal formdaki
sıkıntıları çözmüştük. İkinci normal form ile ortaya çıkan sıkıntıları
çözebilmek için ise nitelikler arasındaki geçişli fonksiyonel bağımlılıkları
ortadan kaldırmamız gerekmektedir.  Bir tablodaki veriden başka bir tabloda
bulunan aynı veri üzerinden ilişkili diğer bir veriye ulaşıp, ulaştığımız
veriyi kullanarak üçüncü bir tabloda farklı bir veriye erişebiliyorsak bu
işlemi geçişli fonksiyonel bağımlılık olarak adlandırırız.  “İl_adı→Posta_kodu”
geçişli işlevsel bağımlılık vardır çünkü bir anahtara bağımlı değillerdir. Bir
anahtara bağlı olmayan geçişli bağımlılıklar tablolara dönüştürüldüğü zaman
Üçüncü normal form(3NF) elde edilmiş olur.

Veri tabanı tasarımında A --> B şeklinde bir fonksiyonel bağlılık bulunuyorsa,
bu bağımlılıktaki B birincil anahtar olmak zorundadır. 3NF tasarımında A
anahtarı bir aday anahtar (candidate key) olmak zorunda değildir. Ancak BCNF’de
bunun tersine A --> B şeklindeki bir fonksiyonel bağımlılık durumunda A bir
aday anahtar olmalıdır Şekil 2.9’daki 3NF göre uygun olan bu tabloda 5100 nolu
öğrenci silindiğinde


