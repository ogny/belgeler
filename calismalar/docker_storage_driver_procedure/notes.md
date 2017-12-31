#### devicemapper kullanmanin ne gibi bir faydasi olur?
standart lvm extend prosedurunden farkli olarak, soyle bir ifade kullanilmis;
Since we are directly accessing the thin pool within docker, we won’t need to
create or extend a filesystem or mount the LVM volumes.
