### Kurulum  
(debian wheezy 7.6 virtualbox) sid ;
```
apt-get install sysdig linux-headers-`uname -r` sysdig-dkms
```
### Kullanim;

By default, sysdig prints the information for each captured event on a single
line, with the following format:

<evt.num> <evt.time> <evt.cpu> <proc.name> <thread.tid> <evt.dir> <evt.type> <evt.args>

where:

· evt.num is the incremental event number
· evt.time is the event timestamp
· evt.cpu is the CPU number where the event was captured
· proc.name is the name of the process that generated the event
· thread.tid id the TID that generated the event, which corresponds to the PID for single thread processes
· evt.dir is the event direction, > for enter events and < for exit events
· evt.type is the name of the event, e.g.  'open' or 'read'
· evt.args is the list of event arguments.

The output format can be customized with the -p switch, using any of the fields
listed by 'sysdig -l'.

#### Filtering 
* sysdig filters are specified at the end of the command line.  
The simplest filter is a basic field-value check:
```
$ sysdig proc.name=cat
```
* Multiple checks can be combined through brakets and the following boolean  
operators: and, or, not.
```
$ sysdig "not (fd.name contains /proc or fd.name contains /dev)"
```

#### Chisels

* debian'da sorunsuz, ubuntu'da cikti alamadigim onemli chisel'ler; ps - netstat - lsof
chisel'ler ile detayli filtreleme yapilabiliyor. Or:
sysdig -c topfiles_bytes "not (fd.name contains /dev or fd.name contains
/proc)"
(/dev ve /proc olmayan en yuksek bytle'li calisan dosyalari goruntule)
* -j ile json cikti alinabiliyor
* -S ile ciktinin sonuna summarize basiyor.
* -w writefile, --write=writefile: Write the captured events to writefile.
* -z --compress; Used with -w, enables compression for tracefiles.

#### Ornekler;
* Print all the open system calls invoked by cat
```
$ sysdig proc.name=cat and evt.type=open
```
* Print the name of the files opened by cat
```
$ sysdig -p"%evt.arg.name" proc.name=cat and evt.type=open
```



* [Kaynaklar](https://github.com/draios/sysdig/wiki/Sysdig%20Examples)



