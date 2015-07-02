### postgres_fdw ile veri tabanina erisim 

#### Case: postgre kullanicisinin, erisim izni olmayan bir veri tabanindaki view'i gorebilmesi.

* kullanici (yoksa) acilir ve superuser yetkisi verilir.
```
CREATE USER oyp SUPERUSER PASSWORD 'password';
SET ROLE oyp;
```
* veritabani (yoksa) acilir, veri tabanina gecilip goruntulenecek tablo (yoksa) olusturulur.
```
CREATE DATABASE ipam_integration;
CREATE TABLE IF NOT EXISTS oyp_nms_node(
id serial PRIMARY KEY,
dat text
);
```
* verinin goruntulecegi vertabanina gecilip fdw ayarlari yapilir.
```
\c ipam_integration oyp
CREATE SERVER oyp_nms_node_server
FOREIGN DATA WRAPPER postgres_fdw 
OPTIONS (dbname 'ttipam', host 'localhost', port '5432');
```

* kullanici map'lenir
```
CREATE USER MAPPING for oyp
SERVER oyp_nms_node_server
OPTIONS (user 'oyp', password 'q1w2e3r4');
```

* Uzak tablo olusturulur
```
CREATE FOREIGN TABLE oyp_nms_node

  (
    region text,
    city text,
    node_name text,
    other_name text,
    host_name text,
    loopback_ip text,
    shelf_type text,
    role text,
    vendor text
  )
SERVER oyp_nms_node_server 
OPTIONS (schema_name 'public', table_name 'oyp_nms_node');
```
* sorgu yapilarak verinin gelip gelmedigi kontrol edilir
```
SELECT * from oyp_nms_node;
```

#### Kaynaklar
* [STO](http://stackoverflow.com/questions/21193642/why-is-postgres-fdw-double-qualifying-schemas)
* [a look at FDWs](http://www.craigkerstiens.com/2013/08/05/a-look-at-FDWs/)
* [PG official docs](http://www.postgresql.org/docs/9.4/static/postgres-fdw.html)
