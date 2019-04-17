#### ruby string interpolation notasyon Ornegi:

* Write the home page.
```ruby
file "#{node['awesome_customers_rhel']['document_root']}/index.html" do
  content '<html>This is a placeholder</html>'
  mode '0644'
  owner node['awesome_customers_rhel']['user']
  group node['awesome_customers_rhel']['group']
end
```
_The "#{}" notation specifies that string interpolation should be performed.
String interpolation enables you to replace placeholders within a string with
the values they represent. Placeholders can be variables or any block of Ruby
code._

##### Ornekler:
```ruby
@var = "Hi"
puts "#@var there!"  #=> "Hi there!"

@@var = "Hi"
puts "#@@var there!" #=> "Hi there!"

$var = "Hi"
puts "#$var there!"  #=> "Hi there!"
```

#### normal_unless
To ensure that the password is generated one time only, you use normal_unless.
normal_unless sets the node attribute only if the attribute has no value.

#### dosyayi_cacheten_cagirma
ayni dosyayi bir cok kez cagiracagin durumda, her defasinda full path vermek
yerine, file_cache_path'e atabilirsin, ornek

# Create a path to the SQL file in the Chef cache.
```ruby
create_tables_script_path = File.join(Chef::Config[:file_cache_path], 'create-tables.sql')

cookbook_file create_tables_script_path do
  source 'create-tables.sql'
  owner 'root'
  group 'root'
  mode '0600'
end
```

#### file ile cookbook_file resource'lari farki
`file` kullandiginda, dosyanin tum icerigini recipe'e yazman lazim.
`cookbook_file`'da ise node'a uzaktaki bir dosyayi transfer edebiliyorsun.

#### template kullanimi
recipe'lardaki code'lari deger olarak template'lere gecebilirsin.
Template'lerdeki placeholder destegi bu degerleri chef-client calistiginda geri
koda donusturur.

#### servisi yeniden baslatma
`notifies` attribute'unu kullaniyoruz, bu sekilde her calistiginda degil,
sadece belirtilen durumda (burada kurulum yapildiginda) yeniden baslatir.

```ruby
package 'php-mysql' do
  action :install
  notifies :restart, 'httpd_service[customers]'
end
```

berkshelf knife cookbook yerine herbir dependency'i manuel olarak yuklemek icin
kullanilir.`berkshelf install` bagimliliklari buraya indirir `~/.berkshelf/cookbooks`
sunucuya yukleme: trusted ssl kurmamak icin `berks upload --no-ssl-verify`


