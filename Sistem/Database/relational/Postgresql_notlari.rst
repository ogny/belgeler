==================
Postgresql calisma
==================

:date: Sal 24 Şub 2015 17:20:20 EET
:comments: true
:categories: 
:tags: 
:Author: Orkun Gunay

* WAL backup'lama point-in-time recovery yapma imkani da sagliyor

* pg_basebackup: binary dosya, tum db'leri birarada alabiliyorsun

  * pg_ctl'i kullanabilmek icin .bash_profile'a eklenecek path::

    /usr/pgsql-9.4/bin/

* veri tabani kullanicisiyla baglanmadiginda aldigin hata
pdns=> DROP DATABASE pdns;
ERROR:  must be owner of database pdns

* cozum;
# psql -U postgres -c "drop database pdns"
DROP DATABASE

* yeni veri tabani - kullanici ekleme
CREATE USER kullanici_adi WITH PASSWORD '';
CREATE DATABASE db_adi;
GRANT ALL PRIVILEGES ON DATABASE db_adi to kullanici_adi;

* auto-increment'i arastir.


#. Drop all tables in postgresql?

drop schema public cascade;
create schema public;

http://stackoverflow.com/questions/3327312/drop-all-tables-in-postgresql

DROP TABLE -- remove a table

#. Dump ::

    pg_dump -U <veri_tabani_sahibi> <veri_tabani_adi> -f dosya.sql
    pg_dump dbname > outfile
  
#. Restore::

   psql -U <veri_tabani_sahibi> <veri_tabani_adi> -f dosya.sql
   psql dbname < infile


#. kullanicinin parolasini guncelleme ve superuser yetkisi verme::
   ALTER USER <kullanici_adi> WITH PASSWORD '<newpassword>';
   ALTER USER <kullanici_adi> WITH SUPERUSER;

#. Tablo olusturma calismasi::

    create table employees(
    id int PRIMARY KEY,
    last_name varchar NOT NULL,
    salary int
    );
    INSERT INTO employees VALUES (1,'Jones',45000);
    INSERT INTO employees VALUES (2,'Adams',50000);
    INSERT INTO employees VALUES (3,'Williams',37000); 

Kaynak
======

`Views <http://postgresguide.com/SQL/views.html>_`
`CREATE FUNCTION: return types <http://www.postgresqlforbeginners.com/2010/11/create-table-and-constraints.html>_`

