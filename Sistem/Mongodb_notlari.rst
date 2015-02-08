====================================
Mongo Python console calismasi
====================================

:date: 2015.02.01

#. MongoDB is a document database. This means that we store data as documents,
   which are similar to JavaScript objects. Here below are a few sample JS
   objects:

#. mongo'yla Connection ile konusuyoruz.

#. Collection relational db'lerdeki table'a denk dusuyor.

#. mongodb console'da it pagination yapiyor.

#. Mongo'nun geospatial index'i var: 

#. graph database'ler var; bunlarda tablolar vs. hicbir sey yok sadece
   noktalarla baglantilar var (neo4j)
   ayni yerdeler - ayni seyleri begeniyorlar vs. baglantilar kuruluyor.

#. sadece log kaydedilen db'ler var: time series db: tarihlerden tolerans
   vererek tum verileri egalate etmenizi sagliyor.

#. sorgulari siralama

..code:: sh

    db.scores.find({a: {'$gt': 15}})


#. objectId random olarak uretiliyor (UUID)

#. save ile insert arasinda fark yok

#. mongo update'te $addToSet ile yeni ekledik.


