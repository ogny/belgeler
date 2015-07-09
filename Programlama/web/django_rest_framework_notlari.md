### [Django REST framework](www.django-rest-framework.org/)

Django icinden Restful API kullanimi icin utility.
Web servisi ile Web application farklari;
* Web service'de mantik: on yuz uygulamasi ne olursa olsun (browser, mobile
client, curl vd.) bu servislere erisebilsin. Web application html'i render
ettiginden platform-based
* Web service bir seyi bir yere redirect etmez, hep bir request/input alir,
response/output dondurur. 

##### HTTP Metodlari

* `GET`: Server'dan bilgi alir. Browser her zaman GET yapar. 

* `POST-PUT`: Input gonderme; yontemleri
    - path-variable /foo/bar
    - request parametreleri (query string)

Yeni bir obje yaratacaksan `POST`
Varolan bir objeyi guncelleyeceksen `PUT`

Django konseptlerinin DRF'deki karsiliklari;
View - view ve viewset
form - serializer

Django'da form input'lari gorsellestirmeyi, datayi isleyebilmeyi sagliyordu.

