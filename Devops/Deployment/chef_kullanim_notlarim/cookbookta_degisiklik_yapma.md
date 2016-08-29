#### firewall'a yeni kural ekleme

bunu neden yapıyoruz? örneğin bir web app'i ekliyoruz, default olarak
port'lar kapalı. Amacımız 80'i açmak ve geri kalan kuralların aynı
kalmasını sağlamak.  Bunun için  firewall cookbook'una, bir nevi kendi
kuralımızı (80'i aç) inject edeceğiz.

* olusturdugumuz cookbook'a firewall cookbook'unu depend edelim.
```
vim cookbooks/awesome_customers_rhel/metadata.rb
depends 'firewall', '~> 2.4.0'
```

* cookbook'ta firewall recipe'i oluşturalım
```
chef generate recipe cookbooks/awesome_customers_rhel firewall
```

* cookbook'ta default attribute'u oluşturalım ve icine yazalım. 

  - ilk 2 satirda firewall cookbook'unu kendi cookbook'umuzun ekli
    olduğu yerlerde override edeceğiz. Son satırda kendi kuralımızı
    firewall cookbook'unu kullanarak inject ediyoruz.
```
chef generate attribute cookbooks/awesome_customers_rhel default
vim  cookbooks/awesome_customers_rhel/attributes/default.rb
default['firewall']['allow_ssh'] = true
default['firewall']['firewalld']['permanent'] = true
default['awesome_customers_rhel']['open_ports'] = 80
```

* oluşturduğumuz firewall recipe'ına açmak istediğimiz port'un
  kuralını yazalım. Burası attribute'taki tanımı kullanıyor. (Dinamik) 
```
include_recipe 'firewall::default'

ports = node['awesome_customers_rhel']['open_ports']
firewall_rule "open ports #{ports}" do
  port ports
end

firewall 'default' do
  action :save
end
```

[Kaynak](https://learn.chef.io/manage-a-web-app/rhel/configure-the-firewall/)


#### web admin Kullanıcısı oluşturma



[Kaynak](https://learn.chef.io/manage-a-web-app/rhel/create-the-web-admin-user/)
