Redis cluster
=============

Cluster komutlari
~~~~~~~~~~~~~~~~~

::

    redis-trib.rb del-node     
    redis-trib.rb check

#. Yapilacak: cluster docker images'ta calistirilip test edilecek.



Arastirma:
~~~~~~~~~~

cluster'daki inconsistency


* Redis Cluster does not use consistency hashing, but a different form of
  sharding where every key is conceptually part of what we call an hash slot.

* Yeni not eklemek istedigimizde, diger node'lara tanimladigimiz hash slot'lari
  yeni node'a kaydiriyoruz. boylelikle operasyonel duzeyde islem yapmadan
  (durdurma vb.) node ekleyip cikartabiliyoruz.

* The user can force multiple keys to be part of the same hash slot by using a
  concept called hash tags.

* Redis Cluster specification: if there is a substring between {} brackets in a
  key, only what is inside the string is hashed. Bu durumda; bu{foo}key ile
  diger{foo}key 'in ayni hash slot icerisinde oldugu garanti altindadir.

YAPI
----

* Her VM icerisinde bir master bir diger master'in slave node'u barindirilsin.
  A -> B1 gibi. Bu durumda hem node'lardan biri veya VM'in erisilemez oldugu
  durumda cluster bozulmadan calismaya devam edebilir.

* Node B1 replicates B, and B fails, the cluster will promote node B1 as the
  new master and will continue to operate correctly.  However note that if
  nodes B and B1 fail at the same time Redis Cluster is not able to continue to
  operate.



