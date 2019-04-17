zone transferinin izinli olmasi bir nevi guvenlik acigi olarak goruluyor, default olarak engellemeli
In named.conf file within the "options" section add:
allow-transfer {"none";};

lokal dns'le dis dns'i ayirma
forwarded zone
lookupforward
(bunun nasil yapildigini bilmiyorum, calisacagim.)

* sorgulanan domain uzerinde degilse git baska bir dns sunucuya sor demek icin
  `recursion no; allow-transfer { none; };` dedim, laptopu farkli bir networke
goturdugumde test edecegim
