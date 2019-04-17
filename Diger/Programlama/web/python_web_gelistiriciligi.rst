degisken atama
yas = 20
yas = "yas" (string'ler cift tirnak, sayi da olur ama ayni deger degiller.)
"20" + 10 hata verir. (iki ayri tiple birlikte calismaz)

diger veri tipleri de var. float

python listeleri (diger dillerdeki array)
[]
ilk eleman sifirinci

java'da php'deki interface 

array ayri bir kutuphane python'daki liste array'den daha gelismis.
listeyle python ayrimi, farkli veri tiplerini birarada tutabiliyorsun


yeni eleman eklemek icin append 

print len(liste_adi)

python'da dongu icin for'daki gibi 1'den 10'a dondurmek icin
range ile iterate ediyorsun

for i in range(2,10);
    print $i
    
#. pythonn'da if dongusu diger dillerdeki gibi.

if isim=="dogan";
    print "ye"

syntax hatasi strict oldugu icin yeni bir blok acip ileride doldurmak istersen
"pass"i kullan.

if isim=="dogan";
    pass
else
    print "okey"

#. listelerde sik kullanilan bir durum, liste icinde liste tutma;

insanlar =  [ [ 22, "can"],
            [ 30, "ben"],
            [ 25, "sen"],
            [ "sayi", 13 ] ]

( for dongusu biraz karisik geldi, bunu calisalim)

#. listelerde indis'ler;

    #. python'da indis alirken ilk karakteri python bildigi icin [0:3] yazmana gerek
yok, [:3] yeterli

#. Son uc karakteri almanin yolu 

    #. sayilar[-3:]   

#. sayilar[::] birer birer atlayarak gitme (listenin tekrari aslinda)

    sayilar[::]  bunu bir listeyi kopyalamak icin yapiyoruz ( ayni listeyi
    degisken olarak atasak kendisini linkliyor, ama biz kopyasini istersek bunu
    kullaniyoruz. (aksi durumda birinci listede degisiklik yaparsan
    linklenen liste de degisiyor.)

Fonksiyonlar
------------

def topla(s1,s2):

    return s1 + s2

def fun(x, y=3)

burada y=3'u atadik, eger buraya bir deger vermezsek default y=3'u alacak,
verdiysek o degeri atayacak.

python'daki fonksiyonlar da bir obje; uzerine bir sey yaz, parametre olarak
kullan. Fonksiyonlarin first class olmasi. fonksiyonlar da ayni string gibi
integer gibi bir deger.

design pattern'de fornksiyonun bir parametresi baska bir fonksiyonu cagiriyor.
bu sekilde kurgula. isin nasil yapilacagini.

#. fonksiyonlar ve string'lere bakalim; ikisi de python'da bir veri;
   fonksiyonlarin farki cagrilabiliyor olmasi, string'in farki concanetate
   olmasi.

#. sozluk'u key-value seklinde bir deger almak icin kullaniriz, siralama
   onemliyse liste kullan.

#. tuple basiyor?

#. help(sozluk.get)

deger girerken input ve raw_input farki, her zaman raw_input al, kendin onu
integer'a  cevir, kullanicinin sayi girip girmedigini kontrol edip hata dondur,
ama kullanicidan input isteme.

#. raw_input 'la al, if'le bakip print bas table dondur;

#. Classes, Objects

ne icin class yaziyoruz? bize ne saglar?

butun string'lerin ortak bir ozelligi var degil mi? iste bu string bir class
"armut" string sinifindan yaratilmis bir obje

str class'i string objelerinin temel ozelliklerini gosteriyor.
"elma".upper() 
"armut".upper() gibi

ortak hangi islemleri yapabilecegimi gosteriyor.

ornegin dictionary'ler uzerinde yapabileceklerim
dict class'inda keys objesi

ornegin: sayi 

python'da class yaratalim; (bos bir blok tanimlayalim)

class Window(object)
    def __init__(self,title):

        self.title=title
        self.xpos=0
        self.ypos=0

    def maximize(self):
        
        self.width=screen.width
        self.height=screen.height

w1=Window()
w2=Window()

metod dedigi, class'in icine yazacagi fonksiyonlar
bu fonksiyon ilk aldigi sey yaratilan nesne olacak, bunun vasitasiyla o nesneye
erisebilecegiz.

class tanimlayarak kendi kendimize bir islem sistemi (mesela toplama islemi)
olusturuyoruz. Dahasi artik bu gelistirdigimiz sistemi her seferinde tekrar
etmemize gerek kalmiyor.
 
bir soyutlama sagliyor. Artik arka tarafta yapabilecegin islemleri bilmen
gerekmiyor. 
birinci parametre

(w1 yaratiyorum, ayni fonksiyon cagirir gibi cagiriyorum.)

def metodda da fonksiyonda da def

#. Inheritance

  class person(student):

ama tersi mumkun degil.

neden class Window(object) yaziyoruz? hiyerarsinin en tepesinde object var.

Moduller
--------

#. aslinda her dosya bir moduldur. bunu baska bir yerde modul olarak
   kullanabilirsin.

    #. calistigimiz dosyayla ayni dizindeyken import ile o dosya da gecerli hale
    gelir.
   
    #. Dogrudan ulasabiliyoruz,tum icindekileri alabilir import * 

#. bir klasoru python modulu haline getirmek icin;
   eger dizin icerisinde __init__.py dosyasi varsa bunu python modul klasoru
   olarak degerlendiriyor. 

import human dediginde namespace kavrami geliyor
cagirirken human.age ile cagiriyoruz

from  human import age dediginde age ile cagir.

#. Projenin requirements.txt 'sini olusturuyoruz. asagidakiyle kuruyoruz.

sudo pip install -r requirements.txt

#. Kullaniciyi degistirip virtualenv'e gectiginde artik geerli include'lar
   bulunmayacak.

freeze yaptiginda burayi alan biri aynisini alabilecek.
   
  stormssh ne ise yariyor?

#. api benzeri html ciktisi uretmeyen islerde micro framework kullaniliriz.


#. map'in kullanimi;
   def'le bir fun tanimlamasi yap
   lambda'yi kullan 
   map'la (func,iterable)

list comprehension'la ayni isi bu da yapabiliyor.

lambda for-while gibi bir atanmis kelime
ic ice if'ler yazabiliyorsun
map'te bunun karsiligi filter
verdigimiz fun'dan sadece true donenleri aliyor.
filter(lambda x:x%2, range(100))

buyuk bir text'te benzersiz kelimeleri yakalayan ve belli bir sayidan fazla
(histogram uygulamasi)

#. dosya okuma
f= open ("...", "read veya write") modunda
f= read
f= readlines()

#. strip metodu (r ve s ile basindaki sonundakileri siliyor.)
#. split metodu
   kelimeyi istedigni ayraclara gore bol (bosluga, virgule vb.)

#. my_list.count("foo") --> bu karakterin kac kere gectigini yakalar.
#. kelimelerin baslarindaki sonlarindaki karakterleri silecek

herhangi bir textten bir dictionary nasil olusturulur (dict'e atarsin)

#. python'da bir karakteri istedigin kadar buyutme

   '*' * 10
   '*' * 20

#. split'e parametre vermezsen butun bosluklari siler.

#. Flask'in view'lerine baslangic;

NOT: tmux session'lari kaydetmek lazim oldu.

app.route("/") --> sayfanin acilacagi root
/ 'mis yani alt sayfa yok
altinda /hello tanimlanmis

request'lerde post ve get

form'da get olarak belirtirsen url'de gorursun

conversion olarak;
bir sey siliyorsan, yapiyorsan post
bir sayfaya git vs. diyorsan get kullan

HTTP Methods

request metodunu kullan, kullanici ne yapiyorsa ona gore
mesela bir post'un var, GET'le gelirse icerigi donersin.

POST'la gelirse buna yorum geldi mesela, buradaki ayrim request'ten farkli bir
seyler istiyor.

statik dosya : gelen istekelre gore icerik uretiyoruz, geldigi url'e gore

statikler hic degismeyecegi icin bunlari kullaniciya cache'lettiriyoruz
(herhalde nginx'ten)

routing'in anlami bu;
buraya istek geldiginde fonksiyonu cagiriyor   

bir blog uygulamasi yaparken bunu db icerisinden yapabiliriz.

kucuk not: for dongusu cok zevkliydi.

python programinda listede tuttugun veri programi kapattiginda kapanir.
Mongo'ya yazarak veriyi koruyoruz.

pip freeze ile kurulmuslari gorebiliyoruz, 
-r requirements.txt ile projeyi ayaga kaldir.
burasi istedigin parametreyi alir. freeze'i biraz daha incelemekte fayda var.

mysql'den farki 
tablo - collection
dokuman - kayit, satir (python'daki dictionary)
her kayyitta tuttugun veri tipi hep ayni oluyor (basinda olustururken)
bir sonrakine baska bir sutun ve veri tipi ekleyebilirsin

her sey on-the-fly tablo olusturmuyorsun, bir veri girdiginde her seyi kendi
olusturuyor. 

#. notasyon'da ikisi de olur:
db = Connection()["akademikbilisim"]
db = Connection().akademikbilisim

db.people.insert({"name": "Orkun"})
db.people.find()
list(db.people.find())

adam get'le geliyorsa bir sey yazmayayim, direk ekrana basarim (find'le alip
for dongusuyle ekrana basarim)

representational state transfer
2 servisi birbiri arasinda konusturmak icin kullanilir.

rest'in bazi convention'lari
get'le bir sey istediginde json dondurur, veri tabanina bir sey yazmaz.

api'de post gonderirken json gonderebiliyorsun
(payload )

GET / (bir resource'un kokune indiginde listelemis olursun)
POST /<id>  --> detay
POST /  --> yeni ekleme
PUT /<id> --> guncelleme
DELETE /<id> --> silme

bir tane id aliyoruz, mesela mongodb id'si gibi.

status codes
yeni post ekleme 201 (hata varsa 400 bad request)
put 202
silme 204 olabilir

gelen ciktiyi kabul etmek icin validation'u kontol etmen gerek

jsonschema diye bir program var;
bu standartlastiriyor.

mongodb.org dokumantasyon

try.mongodb.org'dan calisabilirsin.

#. mongo'da python'la calisabilecegimiz func.

find() => parametre vermezsen hepsini getirir.
          Kullanilabilecek metod'lari:
          #. skip =>  pagination yapmak icin kullanilir.
veri tabanindaki tum kayitlari listelemeyecegiz, kullanicinin gorecegi 20'ser
kayit dondurur.( kayitlardan 20den sonrasina atla, 20'den sonrasini getir.)
          #. limit => 20'den sonraki 10 kaydi getirir.
Or: skip0 limit 20

update() => diyelim blogpost'un basligini guncelleyeceksin; id'sini veriyorsun,
guncellenecek field'lari veriyorsun. burada hata olma olasiligi var: (klasik
veri tabanlarindaki gibi guncellenmiyor. => $set vs.  demeden yaparsan 

remove()
insert()

#. mongodump mongorestore (tum veri tabanlarinin dump'ini alma - acma)

#. mongo'da save ile insert ayni (try.mongo'da save geciyor.) => bunlari
   ipython'dan yapmayi dene.

yapmak istedigim:
hem kullaniciya dondurecegim hem de db'ye kaydedecegim
mongodb gonderdigimiz veride bir degisiklik yapti. ekledigi _id degerini bizim
verimize de ekledi, jsony burada serialize olmadigini soyledi.
Cozum: id'yi data'dan sil, jsonify duzeldi.

null donduruyordu, name="x" message="y" gonderdim.


flask'i import et

api response'umuz jsonify'i import et

hello world'u al
app'tan sonra
db = Connetion()['thewall']
db.messages adli bi kolleksiyon olusturdu (konsol'dan)
l adli bi liste olusturdu konsolda.
a = [] diye bi liste olusturdu
db.messages.insert(a) diye icinde yazdi
list ile listeledi

bu datayi api'den nasil cekerim

post - get e bak
/api/messages

fonksiyonda
    messages= db.messages.find()

    result = []
    
    
    for message.in.messages:
    result.append(a)

"konsoldan yeni bir insert girdi)

return jsonify(messages=[
json girdisi
])
json'un icine bir dict, liste, vs. yazabilirsin. (istedigin veriyi modelleyebiir,sunabilirsin)

#. veri girme:
db.messages.insert({"name":"ali", "message":"slm"})

#. form'la tarayici uzerinden istek gonderiyorduk, api'yi kullanan adam
post-get istek gonderiyor, httpclient'iyle, bizim de test icin curl veya
python'in httpie aracini kullanabilirsin.

httpie ile istek yaparken default'u get

http post google.com yaptiginda (method not allowed)

#. sunucuda post tanimi yapmadigimizdan post gonderdigimizde 405 aldik.

#. get'te de uzun post'ta da uzun islemler yapiyorsan 2sinde ayri islem
yaptigimiz icin bunlari bolmekte fayda var. flask bize bunu sagliyor;
method'lari boluyoruz.

yeni bir app altinda post'a return dondurduk.

ortada bir form yok ama biz http'ye gore form verisi gonderdigimizde form
olmamasi bir seyi degistirmez
http -f post ile gonderilir

#. simdi bir veri kaydettik, adama da bunu kaydettigimizi json olarak donelim.
DB'ye kaydettigimi kullaniciya nasil gosteriyorum?

bir degiskene atayip jsony'i retun ederiz.

hepsini almak icin find().
siralamak icin find().sort(
    [(date-send, -1)]

#. artan istiyorsan descend +1
#. azalan icin ascend -1




