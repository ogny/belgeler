### Tips & Tricks

Ansible ne yapar: node'lara bağlanıp playbook'lardaki 'ansible modül' denilen
programları çalıştırır. İş bitiminde de bunları siler

tasks:
   - shell: some tasks go here
   - meta: flush_handlers
   - shell: some other tasks

tum task'lari yazdigin dosyaya playbook deniyor.

galaxy'den cekebiliyorsak fork'layip kullanalim. --> 
olmuyorsa internal_role olusturucaz.

proje-musteri-alt_bilgi: 
bir bir env. yonetiyorsak, onun conf.'unu versiyonlamaliyiz.
or: proje-musteri-alt_bilgi

inventory'de host-grup iliskisini tutuyoruz. burada variable vermiyoruz.

ansible-playbook -vvvv -i 'localhost,' -c local $(pwd)/tests/main.yml

ansible.cfg duzenlemeyi unutma
[defaults]
roles_path = ~/Code/ansible_roles 
ve hosts path

* Default structure'i olan bir role olusturmak:

```bash
ansible-galaxy init role_name
```

* external role'leri belli bir dizin altina cekmek
ansible-galaxy install -p <dizin_yolu> <role_adi>

### Inventory
Inventory INI formatinda `/etc/ansible/hosts` dosyasinda tutuluyor.

* grup ve host variable'larini hosts dosyasina yazmaktansa alt gruplar
  olusturmakta fayda var
* group_vars/ ve host_vars/'in her ikisi de olusturulursa playbook 
  altindaki git'te tutulmasi oneriliyor.
* `serial` parametresiyle rolün kaç sunucuda çalışacağını belirtiyorsun.

an example app_config.yml might look like:
```yaml
 ---
- yum: name={{contact.item}} state=installed
with_items:
- app_server
- acme_software

- service: name=app_server state=running enabled=yes

- template: src=/opt/code/templates/foo.j2 dest=/etc/foo.conf
notify: 
- restart app server
```



#### Kurulum: 
* ubuntu precise x86 on vagrant
```
sudo apt-add-repository ppa:rquillo/ansible && \
sudo apt-get update && \
sudo apt-get install ansible && \
wget https://ftp.dlitz.net/pub/dlitz/crypto/pycrypto/pycrypto-2.6.tar.gz && \
tar -xf pycrypto-2.6.tar.gz && \
cd pycrypto-2.6 && \
sudo python2.7 setup.py install && \
```

* centos 6.7 on vagrant
```bash
Ansible now needs setuptools in order to build. Install it using your package manager (usually python-setuptools) or via pip (pip install setuptools).
Setting up Ansible to run out of checkout...
PATH=/root/ansible/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
PYTHONPATH=/root/ansible/lib:
MANPATH=/root/ansible/docs/man:
Remember, you may wish to specify your host file with -i
```



## Kurulan ek uygulamalar
* ansible-doc
* ansible-galaxy
* ansible-playbook
* ansible-pull

## Yapilandirma

## Resmi Belgeden Notlar;
* unstable surumun kurulum sebebi;
there is no software to actually install for Ansible itself. No daemons or
database setup are required. Because of this, many users in our community use
the development version of Ansible all of the time,

* python2.x ile calistirilacak.
Python 3 is a slightly different language than Python 2 and most Python
programs (including Ansible) are not switching over yet.

### Ad-hoc commands
An ad-hoc command is something that you might type in to do
something really quick, but don’t want to save for later.  For instance, if you
wanted to power off all of your lab for Christmas vacation, you could execute a
quick one-liner in Ansible without writing a playbook.

command line syntax:
ansible <pattern_goes_here> -m <module_name> -a <arguments>
* -f 10 es zamanli kac process calistirabilecegini belirtiyorsun, bu sayi
ansible.cfg'te default 5, tanimladigin host gruplardaki sunucu sayilarina gore
yeniden tanimlamak gerek.
* -m ile modul calistiriliyor. Eger modul'un calistirilmasi icin shell
gerekliyse shell modul'unu kullan. config dosyasinda tanimli default moudul command
** ansible <pattern_goes_here> -m shell -a 'echo $TERM'
*** Ad-hoc'ta syntax onemli; yukaridaki ornekte cift tirnak
kullansaydik komut karsi sunucudaki kabukta degil kendi makinamizda
calistirilacakti.
* Poll mode: calistirilacak uygulamalarin uzun surmesi durumunda
sinir koyulabiliyor. -B max.sure -P poll statusunun belirtildigi sure
uzun suren komutlarda ve yazilim guncellemede kullaniliyor.
playbook'lar da destekliyor.



### Patterns
Envanterdeki sunucular pattern'ler adiyla gruplandiriliyor.
Pattern'leri tanimlarken asagidaki syntax'tan faydalan;
* : her iki grupta da olabilir.
* :! o grupta olamaz.
* :& kesisimlerinde olmali

#### Handlers
Handlers are best used to restart services and trigger reboots. You probably
won’t need them for much else.

handlers are automatically processed between ‘pre_tasks’, ‘roles’, ‘tasks’, and
‘post_tasks’ sections. If you ever want to flush all the handler commands
immediately, you can:


#### Ansible-Pull
* bare repo ile kulllanilacak



### Yapilandirma dosyalari
Ansible'in sirayla baktigi config'ler
ANSIBLE_CONFIG (an environment variable)
ansible.cfg (in the current directory)
.ansible.cfg (in the home directory)
/etc/ansible/ansible.cfg

#### /etc/ansible/ansible.cfg
* forks: ayni anda kac uzak sunucuyla is gormesi icin belirlenen ust limit. 1.3
surumunden sonra olasi sunucu sayisiyla sinirlandirilmis. Bu deger 500 versen
de 5 sunucun varsa verdigin degerin bir onemi yok.

* If you have multiple hosts following a pattern you can
specify them like this www[001:006].example.com

* logging is off by default unless this path is defined if so defined, consider
logrotate
log_path = /var/log/ansible.log

* list any Jinja2 extensions to enable here:
jinja2_extensions = jinja2.ext.do,jinja2.ext.i18n

* set plugin path directories here, separate with colons
/usr/share/ansible_plugins/

* library: Butun moduller burada tutuluyor, alt dizinler;
cloud     files      messaging           network
commands  internal   monitoring          notification
database  inventory  packaging     	 utilities
web_infrastructure   net_infrastructure  source_control
system

# Diger
## Arastirilacak
* Chef'le beraber gelen vagrant imajinda, vagrant kullanicisini admin grubuna
ekleyip, sudoers'te admin kullanicisini nopasswd yapmislar,
playbook yaml dosyasinda;
sudo: yes
komut satirindan parametre verirken;
--ask-sudo-pass (-K)
Bu durumda sudo ile komutu guvenli bir sekilde calistirilabilir.
"sudo:" you must first disable 'requiretty' in /etc/sudoers
By default, this option is disabled to preserve compatibility with sudoers
configurations that have requiretty (the default on many distros).

### roles_path
The roles path indicate additional directories beyond the ‘roles/’ subdirectory
of a playbook project to search to find Ansible roles. For instance, if there
was a source control repository of common roles and a different repository of
playbooks, you might choose to establish a convention to checkout roles in
/opt/mysite/roles like so:
roles_path = /opt/mysite/roles
Roles will be first searched for in the playbook directory. Should a role not
be found, it will indicate all the possible paths that were searched.

## Gelistirmeler
### Pipelining
(Debian ve ubuntu'da, pipelining icin gerekli requiretty disable durumda.
Bu durumda pipelining'i acabiliriz.
Enabling pipelining reduces the number of SSH operations required to execute
a module on the remote server. This can result
in a significant performance improvement when enabled, however when using

### Calismalar
Ansible agent'larina key-based authentication ile root olarak baglanacak.


## IRC'den notlar
brucelee let's say i want to have a playbook that would connect to DB server as
	 long as the database is runing on the DB server, so it would first log into the
DB server and check the state of the DB, whether its running or not
brucelee can ansible do something like that?
ktosiek  yes, I'm doing something like that
brucelee whats your particular exmaple btw
ktosiek  I'm checking with pacemaker if current server is master, and only apply
	 database/users creation there
brucelee oh can i see your playbook?
brucelee or that part of it that connects to the master, to verify its the master
ktosiek  it connects to all servers and uses crm_resource to find the master (so
	 if you're not using pacemaker it won't help you)
brucelee how do you mkae it connect to other servers though during a playbook
brucelee it runs a "command 'ssh host crm_resource' or something?
ktosiek  http://pastie.org/9315702
ktosiek  no, I just run this playbook on all the DB servers
	 (it's from my general postgresql role)
========================================
- name: Find master
  shell: "crm_resource -W --resource msPostgresql | awk '/Master$/ { print $(NF
- 1); }'"
  always_run: yes
  register: find_master
  until: find_master.stdout
  retries: 15
  delay: 5
  when: not (ignore_master | default(False))
  tags: new_app

- set_fact: postgresql_master_hostname={{ find_master.stdout.strip() }}
  when: not (ignore_master | default(False))
  tags: new_app

# And then I use "when: postgresql_master_hostname == ansible_hostname"
========================================
stickybo I will have to run a command like `find /blah -type d -exec chmod
         755 {} \; -type f -exec chmod 644 {}\;` on it afterwards.
 drybjed check umask
stickybo But when I run with Ansible, it's apparently different.
 drybjed ansible uses /bin/sh by default
 drybjed ln -s /bin/sh ?
 drybjed ansible's json fact interpreter is as strict as ever, one
         misplaced comma and you're done, no facts for you today.
stickybo Even if ansible uses sh, it should still use the default
         umask eh?
stickybo I am using git to clone because `copy` sucks when you have more
         than a few directories/files... and `synchronize` doesn't work
         with sudo's requiretty.
========================================

# Kaynaklar
* https://community.webfaction.com/questions/12333/pycrypto-import-error
* http://docs.ansible.com/
* [ansible-pull](https://github.com/ansible/ansible-examples/edit/master/language_features/ansible_pull.yml#fullscreen_blob_contents)
playbooks are Ansible’s language in YAML format. If Ansible
modules are the tools in your workshop, playbooks are your design plans.
* YAML syntax;
Tum dosyalar ---  ile baslar.
All lines begin with - Each item in the list is a list
of key/value pairs, commonly called a “hash” or a “dictionary”
Ansible doesn’t really use these too much,
but you can also specify a boolean value (true/false)
    * iki nokta ustuste barindiran satirlari tirnak icine al, ornek;
    f̶o̶o̶:̶ ̶s̶o̶m̶e̶b̶o̶d̶y̶ ̶s̶a̶i̶d̶ ̶I̶ ̶s̶h̶o̶u̶l̶d̶ ̶p̶u̶t̶ ̶a̶ ̶c̶o̶l̶o̶n̶ ̶h̶e̶r̶e̶:̶ ̶s̶o̶ ̶I̶ ̶d̶i̶d̶
    foo: “somebody said I should put a colon here: so I did
    * iki nokta ustusteden sonra suslu parantez ile basliyorsa tirnak icine
    al, aksi durumda ansible bunu bir dictionary olarak algilar, ornek;
    foo: "{{ variable }}"

* satir uzuyorsa sonda bos karakter birakip yeni satiri x karakter iceriden
baslatiyoruz.
    tasks:
    - name: Copy ansible inventory file to client
    copy: src=/etc/ansible/hosts dest=/etc/ansible/hosts
        owner=root group=root mode=0644

### Manpage'den 

* -e VARS, --extra-vars=VARS 
Extra variables to inject into a playbook, in key=value key=value  format or
as quoted JSON (hashes and arrays).

* -f NUM, --forks=NUM
Level of parallelism.  NUM is specified as an integer, the default  is 5

* --tags
Run only these tags from the playbook

* --syntax-check
Look for syntax errors in the playbook, but don't run anything

* --diff
template'lerdeki degisiklikleri gosterir, check ile kullanildiginda nelerin
degisebilecegini gosteriyor.

* -c CONNECTION, --connection=CONNECTION
Connection type to use. Possible options are paramiko (SSH), ssh, and local.
local is mostly useful for crontab or kickstarts. 
(local'i anlayamadim, arastiracagim)

### Ansible Modullerinin Man sayfalari
#### authorized_key
/usr/share/ansible/system/authorized_key
Adds or removes authorized keys for particular user accounts
OPTIONS
key    The SSH public key, as a string(required)
user   The username on the remote host whose authorized_keys file will be
modified(required).

Default kullanimi:
    * Example using key data from a local file on the management machine
    - authorized_key: user=charlie key="{{ lookup('file', '/home/charlie/.ssh/id_rsa.pub') }}"

with_file ile kullanimi

    - name: Set up authorized_keys for the deploy user
      authorized_key: user=deploy
                      key="{{ item }}"
      with_file:
        - public_keys/doe-jane
        - public_keys/doe-john

#### add_host
add_host - add a host (and alternatively a group) to the ansible-playbook
in-memory inventory
EXAMPLES
    * add host to group 'just_created' with variable foo=42
    - add_host: name={{ ip_from_ec2 }} groups=just_created foo=42

#### apt
EXAMPLES
    * Update repositories cache and install "foo" package
    - apt: pkg=foo update_cache=yes
    * Update the repository cache and update package "nginx" to latest version using default release squeeze-backport
    - apt: pkg=nginx state=latest default_release=squeeze-backports update_cache=yes
    * Install latest version of "openjdk-6-jdk" ignoring "install-recommends"
    - apt: pkg=openjdk-6-jdk state=latest install_recommends=no
    * Update all packages to the latest version
    - apt: upgrade=dist
    * Run the equivalent of "apt-get update" as a separate step
    - apt: update_cache=yes
    * Pass options to dpkg on run (bunu arastir)
    - apt: upgrade=dist update_cache=yes dpkg_options='force-confold,force-confdef'

### command
Executes a command on a remote node
The given command will be executed on all selected nodes. It will not be processed through the shell, so variables like $HOME and operations like "<", ">", "|", and "&" will not work.
* Example from Ansible Playbooks
    - command: /sbin/shutdown -t now

### copy
The copy module copies a file on the local box to remote locations.  
dest   Remote absolute path where the file should be copied to. If src is a
directory, this must be a directory too.(required)
backup Create a backup file including the timestamp information so you can get
the original file back if you somehow clobbered it incorrectly.  Choices:
yes,no. (default: no)
Example
    - copy: src=/srv/myfiles/foo.conf dest=/etc/foo.conf owner=foo group=foo
  mode=0644
* Copy a new "sudoers" file into place, after passing validation with visudo
    - copy: src=/mine/sudoers dest=/etc/sudoers validate='visudo -cf %s

### crontab
Use this module to manage crontab entries. This module allows you to create
named crontab entries, update, or delete them
Options
cron_file: If specified, uses this file in cron.d instead of an individual
user's crontab.
Examples
* Ensure a job that runs at 2 and 5 exists.
* Creates an entry like "* 5,2 * * ls -alh > /dev/null"
    - cron: name="check dirs" hour="5,2" job="ls -alh > /dev/null"

### easy_install
Installs Python libraries, optionally in a virtualenv 
EXAMPLES
    - easy_install: name=pip



### fetch

       This  module  works  like  copy, but in reverse. It is used for fetching files from remote machines and storing them locally in a file tree, organized by hostname. Note that this module is written to
       transfer log files that might not be present, so a missing remote file won't be an error unless fail_on_missing is set to 'yes'.

### file
Sets attributes of files, symlinks, and directories, or removes files/symlinks/directories. Many other modules support the same options as the file module - including copy, template, and assemble.
OPTIONS 
force  group  mode owner path recurse selevel serole setype seuser src
state

### Filesystem
*  uses mkfs command
* Create a ext2 filesystem on /dev/sdb1.
    - filesystem: fstype=ext2 dev=/dev/sdb1

### Git
* Deploy software (or files) from git checkouts
If  the task seems to be hanging, first verify remote host is in known_hosts.
SSH will prompt user to authorize the first contact with a remote host.  To
avoid this prompt, one solution is to add the
remote host public key in /etc/ssh/ssh_known_hosts before calling the
git module, with the following command: ssh-keyscan remote_host.com >>
/etc/ssh/ssh_known_hosts.
* Example git checkout from Ansible Playbooks
- git: repo=git://foosball.example.org/path/to/repo.git
       dest=/srv/checkout
       version=release-0.22

### include.vars
Loads variables from a YAML file dynamically during task runtime.  It can work
with conditionals, or use host specific variables to determine the path name to
load from.


### kernel_blacklist
Add or remove kernel modules from blacklist.
OPTIONS
name   Name of kernel module to black- or whitelist.(required) 

### modprobe
Add or remove kernel modules
EXAMPLES
* Add the 802.1q module
    - modprobe: name=8021q state=present 

### mysql_db
mysql_db - Add or remove MySQL databases from a remote host.
NOTES
    Requires the MySQLdb Python package on the remote host. For Ubuntu, this
    is as easy as apt-get install python-mysqldb. (See apt.)
EXAMPLES
* Create a new database with name 'bobdata'
    - mysql_db: name=bobdata state=present

### mysql_user
Adds or removes a user from a MySQL database.
EXAMPLES
* Create database user with name 'bob' and password '12345' with all database privileges
    - mysql_user: name=bob password=12345 priv=*.*:ALL state=present

* Ensure no user named 'sally' exists, also passing in the auth credentials.
    - mysql_user: login_user=root login_password=123456 name=sally state=absent

* Example privileges string format
    mydb.*:INSERT,UPDATE/anotherdb.*:SELECT/yetanotherdb.*:ALL

### rabbitmq_parameter
Adds or removes parameters to RabbitMQ 
* Set the federation parameter 'local_username' to a value of 'guest' (in quotes)
       - rabbitmq_parameter: component=federation
                             name=local-username
                             value='"guest"'
                             state=present

### rabbitmq_plugin
Adds or removes users to RabbitMQ
EXAMPLES
       # Add user to server and assign full access control
       - rabbitmq_user: user=joe
                        password=changeme
                        vhost=/
                        configure_priv=.*
                        read_priv=.*
                        write_priv=.*
                        state=present

### Raw
using the shell or command module is much more appropriate. Arguments given to
raw are run directly through the configured remote shell
Should be an absolute path to the executable
EXAMPLES
       # Bootstrap a legacy python 2.4 host
       - raw: yum -y install python-simplejson

### Script
This module does not require python on the remote system, much like the raw
module
EXAMPLES
* Example from Ansible Playbooks
    - script: /some/local/script.sh --some-arguments 1234

### service
Manage services
OPTIONS
    state  Choices: started,stopped,restarted,reloaded..
EXAMPLES
* Example action to start service httpd, if not running
    - service: name=httpd state=started

### Setup
Gathers facts about remote hosts
* Display facts from all hosts and store them indexed by I(hostname) at
C(/tmp/facts).
    ansible all -m setup --tree /tmp/facts

### Supervisorctl
Manage the state of a program or group of programs running via Supervisord
OPTIONS
    state 
    present,started,stopped,restarted
EXAMPLES
*  Manage the state of program to be in 'started' state.
    - supervisorctl: name=my_app state=started
* Restart my_app, connecting to supervisord with credentials and server URL
    - supervisorctl: name=my_app state=restarted username=test
      password=testpass server_url=http://localhost:9001

### Synchronize
Uses rsync to make synchronizing file paths in your playbooks quick and easy.
It does not provide access to  the  full  power  of rsync, but does make most
invocations easier to follow.
* Synchronization of src on the control machine to dest on the remote hosts
    synchronize: src=some/relative/path dest=/some/absolute/path
* Synchronization without any --archive options enabled
    synchronize: src=some/relative/path dest=/some/absolute/path archive=no


### sysctl 
Manage entries in sysctl.conf


EXAMPLES
* Set vm.swappiness to 5 in /etc/sysctl.conf
    - sysctl: name=vm.swappiness value=5 state=present
* Remove kernel.panic entry from /etc/sysctl.conf
    - sysctl: name=kernel.panic state=absent sysctl_file=/etc/sysctl.conf

### template 
Templates a file out to a remote server.
Six additional variables can be used in templates: 
* ansible_managed 
* template_host
* template_uid
* template_path
* template_fullpath
* template_run_date
EXAMPLES
* Copy a new "sudoers file into place, after passing validation with visudo
    - action: template src=/mine/sudoers dest=/etc/sudoers
    validate='visudo -cf %s'

### uri
Interacts with webservices
* Check that a page returns a status 200 and fail if the word AWESOME is not in the page contents.
    - action: uri url=http://www.example.com return_content=yes
    register: webpage
    - action: fail
    when: 'AWESOME' not in "{{ webpage.content }}"

### user
Manage user accounts and user attributes
* Remove the user 'johnd'
    - user: name=johnd state=absent remove=yes
* Create a 2048-bit SSH key for user jsmith
    - user: name=jsmith generate_ssh_key=yes ssh_key_bits=2048

### wait_for
Waits for a condition before continuing.
* wait 300 seconds for port 8000 to become open on the host, don't start checking for 10 seconds
    - wait_for: port=8000 delay=10"

### xattr
set/retrieve extended attributes (setfattr/getfattr utilities)
* Sets the key 'foo' to value 'bar'
    - xattr: path=/etc/foo.conf key=user.foo value=ba
ear

#### Calisilacak
```
ansible-playbook -i inventory -c local --become tests/playbook.yml
```
ansible-playbook -i inventory -c local --become tests/playbook.yml
