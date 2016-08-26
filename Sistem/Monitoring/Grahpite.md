#### Kurulum

###### Ortam

OS: `Centos 6.7`
python: 2.7.2
carbon:
graphite:
grafana: 

###### Kapsam
bir sunucuda toplanan verinin carbon-relay ile diger sunucuya aktarimi ve
aktarilan sunucuda carbon-cache/Graphite/Grafana ile sunumu anlatilacak.

* Gerekli paketler kurulur:
```
yum install -y nginx python-devel pycairo libffi \
libffi-devel bitmap-fonts-compat openldap-devel
```











* email configuration yapilacak, (thresold'larda mail atilmasi icin)
http://graphite.readthedocs.org/en/latest/config-local-settings.html

### Graphing metrics

* buradaki isleri grafana yapiyor, yine de bilmekte fayda var.

* regex kullaniliyor; [...] ile karakter araligi veya liste belirtebiliyorsun;
dash kullanirsan aralik, kullanmazsan liste oluyor
foo[a-z0-9]bar: butun alfanumerik karakterler

* All wildcards apply only within a single path element. In other words, they
  do not include or cross dots `(.)`. Therefore, `servers.*` will not match
  `servers.ix02ehssvc04v.cpu.total.user`, while `servers.*.*.*.*` will.

#### Graph Parameters

#### Kaynaklar

* [official doc](http://graphite.readthedocs.org/en/latest/overview.html)
