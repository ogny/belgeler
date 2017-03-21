#### Audit System Architecture
The Audit system consists of two main parts: the user-space applications and
utilities, and the kernel-side system call processing. The kernel component
receives system calls from user-space applications and filters them through one
of the three filters: user, task, or exit. Once a system call passes through
one of these filters, it is sent through the exclude filter, which, based on
the Audit rule configuration, sends it to the Audit daemon for further
processing.The user-space Audit daemon collects the information from the kernel
and creates log file entries in a log file 

#### Configuring auditd for a CAPP Environment
the Audit daemon must be configured with the following settings:
* The directory that holds the Audit log files (usually /var/log/audit/) should
  reside on a separate partition. 
* `max_log_file` parametresi autdit log files icin ayrilan yerin tamamini
  kullanmasini saglar.
* `max_log_file_action` parametresi `max_log_file` degerine erisildiginden ne
  yapilacagina karar verir, mevcut log'larin uzerine yazilmamasi icin
  `keep_logs` olarak set edilmelidir. 
* `disk_error_action` parametresi diskte hata olusmasi durumunda `syslog`'a gondermesi
  icin tanimlanir.
* `flush` parametresi `sync` tanimlanarak tum event data'nin log'a yazilmasi
  saglanir.

### Rules
three types of Audit rules that can be specified:
* Control rules: audit sistem tanimlamalari ve yapilandirma ayarlariyla ilgili.
* File system rules: (watches olarak da bilinir) audit'in belli bir dosya/dizine erisimi
* system call rules:  sistem cagrisi yapan programlarin loglanmasi.

auditctl ile tanimlanan kurallar yeniden baslatilinca siliniyor. korumak icin
`/etc/audit/audit.rules`'a eklenmeli.

#### Control rules:
* `-r` sets the rate of generated messages per second, for example this
  configuration sets no rate limit on generated messages.
```bash
auditctl -r 100
```
* `-D` deletes all currently loaded Audit rules, for example:
* `-b 8192` Set buffer size
* `-e 2` Make the configuration immutable -- reboot is required to change audit rules
* `-f 2` Panic when a failure occurs

#### File system rules
```bash
auditctl -w path_to_file -p permissions -k key_name
```
* permissions are the permissions that are logged:
  - r: read access to a file or a directory.
  - w: write access to a file or a directory.
  - x: execute access to a file or a directory.
  - a: change in the file's or directory's attribute.

To define a rule that logs the execution of the /sbin/insmod  command, which
inserts a module into the Linux kernel, execute the following command:
```bash
auditctl -w /sbin/insmod -p x -k module_insertion
```

#### System Call Rules
```bash
auditctl -a action,filter -S system_call -F field=value -k key_name
```
* action  can be either always or never. 
* filter specifies which kernel rule-matching filter is applied to the event.
  can be task, exit, user, and exclude.
* system_call specifies the system call by its name. Several system calls can
  be grouped into one rule, each specified after the `-S` option.
* field=value specifies additional options that furthermore modify the rule to
  match events based on a specified architecture, group ID, process ID, and
  others.

#### Ornekler:
* To define a rule that creates a log entry every time the adjtimex or
  settimeofday  system calls are used by a program, and the system uses the
  64-bit architecture, execute the following command:
```bash
auditctl -a always,exit -F arch=b64 -S adjtimex -S settimeofday -k time_change
```

* kullanıcıların belli komutları çalıştırmalarından log üretme:
```bash
auditctl -a always,exit -F path=/bin/ping -F perm=x -F auid\>=500 -F \
auid!=4294967295 -k privileged
```
__Not__: buyuktur isaretine carriage return koyduk. bunu koymadan yapmak icin;
```bash
echo "-a always,exit -F path=/bin/ping -F perm=x -F auid\>=500 -F auid!=4294967295 -k privileged" > /etc/audit/audit.rules 
service auditd restart
```

* kullanici rm,mv,rsync gibi bir komutla dosya/dizinde degisiklik yaparsa 
```bash
auditctl -a always,exit -F arch=b64 -S unlink -S unlinkat -S rename -S renameat \
-F auid\>=500 -F auid!=4294967295 -k delete 
```

#### sorular: 
* auditd kosan bir container, boylelikle root'un dosyalari silmesi
engellenir?
* tum audit'i cmd-audit'e yapistirmali miyiz, incele
* Faydali kurallarin bulundugu dizin: `/usr/share/doc/audit-VERSION`

#### Kaynaklar
* [Redhat Docs: System Auditing](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Security_Guide/chap-system_auditing.html)
* [Linux AUDIT Server](http://www.linuxsv.org/training/l34_linux_audit.html)
