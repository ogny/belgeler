### Yapilandirma dosyasi
/opt/jasperreports-server-cp-5.2.0/apache-tomcat/webapps/jasperserver/WEB-INF/js.quartz.properties

### Baslatma
/opt/jasperreports-server-cp-5.2.0/ctlscript.sh start
/opt/jasperreports-server-cp-5.2.0/ctlscript.sh stop

* Webgui'den raport olusturma;
http://<ip>:8080/jasperserver/flow.html
ipam/general/ip_conflict_v2_list'in yanindaki saaatte tikla
sql'i almak icin en sagdaki edit'e tikla
next
* sql_query'i al
cancel
run now
yeni olusturup sonuna limit 100 ekle
Output Identification'da basina `test_` ekle
owerrite files
attach files
job name'de Background Execution olarak gorunuyor

* jasper server surumunu ogrenme: 
```
/opt/jasperreports-server-cp-5.2.0/apache-tomcat/webapps/jasperserver/WEB-INF/internal/jasperserver.properties
```

* [5.2 icin kurulum dokumani](https://community.jaspersoft.com/sites/default/files/docs/jasperreports-server-install-guide_2.pdf)

* classpath: 
```
/opt/jasperreports-server-cp-5.2.0/apache-tomcat/webapps/jasperserver/WEB-INF/lib
burasi da olabilir: /opt/jasperreports-server-cp-5.2.0/apache-tomcat/bin/
```

* postgre'yi standalone calistirma:
```
/opt/jasperreports-server-cp-5.2.0/postgresql/bin/pg_ctl \
-D /opt/jasperreports-server-cp-5.2.0/postgresql/data \
-F -p 5432 restart -m fast
```
##### Veri tabani sunucusunu lokal'den uzak sunucuya tasima

* tasinacak veri tabanlari
```
foodmart     | jasperdb | UTF8     | C         | C     |
jasperserver | jasperdb | UTF8     | C         | C     |
sugarcrm     | jasperdb | UTF8     | C         | C     |
```
