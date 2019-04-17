#### role taniminda json olarak belirtilir

```bash
knife role create <rol_adi>
```
```json
 "default_attributes": {
    "nginx": {
      "log_location": "/var/log/nginx.log"
    }
  },
  "override_attributes": {
    "nginx": {
      "gzip": "on"
    }
  },
```

* belki bu sekilde de tasarlayabilirsin;
```bash
knife role from file FILE
```
```json
default_attributes "nginx" => { "log_location" => "/var/log/nginx.log" }
override_attributes "nginx" => { "gzip" => "on" }
```

override edemezsen ayrica kullanabilecegin bir resource daha var;
A `force_override attribute` located in a cookbook attribute file

bazi durumlarda tanimlama degisiyor, bu farki ayirt etmek iyi olur.
```
default[abc][xyz]
node[abc][xyz]
```

### attribute'ler hangi durumda nasil kullaniliyor;
* Examples
The following examples are listed from low to high precedence.

  - Default attribute in /attributes/default.rb
```json
default['apache']['dir'] = '/etc/apache2'
```

  - Default attribute in node object in recipe
```json
node.default['apache']['dir'] = '/etc/apache2'
```

  - Default attribute in /environments/environment_name.rb
```json
default_attributes({ 'apache' => {'dir' => '/etc/apache2'}})
```

  - Default attribute in /roles/role_name.rb
```json
default_attributes({ 'apache' => {'dir' => '/etc/apache2'}})
```
  - Normal attribute set as a cookbook attribute
```json
set['apache']['dir'] = '/etc/apache2'
normal['apache']['dir'] = '/etc/apache2'  #set is an alias of normal.
```
  - Normal attribute set in a recipe
```json
node.set['apache']['dir'] = '/etc/apache2'
node.normal['apache']['dir'] = '/etc/apache2' # Same as above
node['apache']['dir'] = '/etc/apache2'       # Same as above
```

  - Override attribute in /attributes/default.rb
```json
override['apache']['dir'] = '/etc/apache2'
```

  - Override attribute in /roles/role_name.rb
```json
override_attributes({ 'apache' => {'dir' => '/etc/apache2'}})
```
  - Override attribute in /environments/environment_name.rb
```json
override_attributes({ 'apache' => {'dir' => '/etc/apache2'}})
```

  - Override attribute in a node object (from a recipe)
```json
node.override['apache']['dir'] = '/etc/apache2'
```

_Not_: node.set is deprecated and will be removed in Chef 14, please use node.default/node.override (or node.normal only if you really need persistence) 
