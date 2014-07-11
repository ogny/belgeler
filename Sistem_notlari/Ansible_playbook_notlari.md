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
*# Ensure a job that runs at 2 and 5 exists.
*# Creates an entry like "* 5,2 * * ls -alh > /dev/null"
- cron: name="check dirs" hour="5,2" job="ls -alh > /dev/null"

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



