## Test edilen ortam: ubuntu precise x86 on vagrant

## Kurulum: (unstable)
```
sudo apt-add-repository ppa:rquillo/ansible && \
sudo apt-get update && \
sudo apt-get install ansible && \
wget https://ftp.dlitz.net/pub/dlitz/crypto/pycrypto/pycrypto-2.6.tar.gz && \
tar -xf pycrypto-2.6.tar.gz && \
cd pycrypto-2.6 && \
sudo python2.7 setup.py install && \
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

### Inventory
#### /etc/ansible/hosts
* SSH port default olmadiginda belirtilir. :2222
* grup ve host variable'larini hosts dosyasina yazmaktansa alt gruplar
olusturmakta fayda var;
** group_vars/ ve host_vars/'in her ikisi de olusturulursa playbook altindaki
variables'lardan oncelikli olurlar.
** git'te tutulmasi oneriliyor.

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

tasks:
   - shell: some tasks go here
   - meta: flush_handlers
   - shell: some other tasks

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
