### Synopsis:
[set of characters]{number of characters to match}


#### Karakterler
*  ^ baslangic satiri karakteri
    Ornek:
        ^A
    A karakteri ile baslayan satirlari listele

*  \ baslangic karakteri
    Ornek:
        \A
    A karakteri ile baslayan string ifadelerini listele

*  \t tab
*  \s space

*  [] karakterleri arasinda
    Ornek:
        [A-Z]
    A ile Z karakterleri arasinda, olanlari alir.

*  . herhangi bir karakter

* {} arasinda belirtilen kadar karakter al
    Ornek:
        {1,5}

* \b kelime sinirindan kurtar, ornegin a ile baslayan yakalamak icin;
\ba[a-z][0-9]+\b

literal A literal is any character we use in a search or matching expression,
for example, to find ind in windows the ind is a literal string - each
character plays a part in the search, it is literally the string we want to
find.

metacharacter   A metacharacter is one or more special characters that have a
unique meaning and are NOT used as literals in the search expression, for
example, the character ^ (circumflex or caret) is a metacharacter.

Köşeli parantezler arasindaki metacharacter'ler;
* tire: aralik belirler; Or: [A-z0-9] tum alfanumerik karakterlerle eşleşir.
** ^ (circumflex or caret) tanimi olumsuzlar; Or:  [^Ff] Buyuk ve Kucuk f
disindaki her karakterle eşleşir.

Positioning (or Anchors)
* ^ koseli parantezlerin disindaysa kendinden sonra gelen ilk karakterden
itibaren baslandigini belirtir.
* $ kendinden onceki karakterin son karakter oldugunu belirtir.
* . bulundugu yerdeki karakteri yakalar, sondaysa string'in
sonuna kadar gelecek karakteri, bastaysa string'in basindan gelebilecek
karakteri yakalar.

### Iteration 'metacharacters' (a.k.a. quantifiers) 
* ?   kendinden onceki karakter sifir veya 1 kere tekrar ediyorsa yakalar.
* "*" kendinden onceki karakter sifir veya sonsuz kere tekrar ediyorsa yakalar
* +   kendinden onceki karakter 1 veya sonsuz kere tekrar ediyorsa yakalar
* {n} kendinden onceki karakter veya karakter araligi, n kere tekrar ediyorsa
yakalar. mesela cep telefon numarani ele alalim; (simdilik bosluktan kacmayi
bilmedigimden araya tire koyuyorum) 532-416-29-51
[0-5]{3}-[1-4]{3}-[2-9]{2}-[1-5]{2}
* {n,m} kendinden onceki karakter veya karakter araligi en az n kere
tekrarlayabilir ama m'den fazla kere tekrarlayamaz.
* {n,} kendinden onceki karakter veya karakter araligi en az n kere
olmak üzere sonsuz kez tekrarlayabilir.
    



















