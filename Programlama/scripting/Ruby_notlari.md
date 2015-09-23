============
Ruby notlari
============


Time Module
-----------

strip module
------------


* oci8 ile oracle veri tabanina erisim
```
/opt/sensu/embedded/bin/ruby -r oci8 -e "OCI8.new('<kullanici_adi>', '<parola>', '//<ip>:<port>/<db_adi>').exec('SELECT * FROM <tablo>') do |r| puts r.join(','); end"
```


* gem ile kurulu bir paketi silme: `cleanup`
* spesifik gem versiyonu kurma `--version`
