server ve client icin; syslog-ng paketi kurulumda rsyslog paketini kaldiriyor

### Tanimlar
* A log path consists of one or more sources and one or more
destinations; messages arriving from a source are sent to every destination
listed in the log path. A log path defined in syslog-ng is called a log statement.

* The Premium Edition of syslog-ng can store messages on the local hard disk if
the central log server or the network connection to the server becomes unavailable. 
This feature is called the disk buffer and needs to be configured only on the client side.
( ucretli surumde bulunan bir ozellik.)

* The syslog-ng PE application can send and receive log messages in a reliable
way over the TCP transport layer using the Reliable Log Transfer Protocol (RLTP).
RLTP is a proprietary transport protocol that prevents message loss during connection breaks.
( ucretli surumde bulunan bir ozellik.)

### kurulum sonrasi yapilandirma
* Dizin-dosyalar;
```
ls /etc/syslog-ng 
conf.d  modules.conf  patterndb.d  scl.conf  syslog-ng.conf
```

* Macros in filenames
mesajlarin tarih duzeninden gelmesi icin;
```
destination d_messages 
    { file("/var/log/messages"); 
    file("/var/log/$YEAR/$WEEK/$HOST-messages" create-dirs(yes));
    };
```

### Sorular;
* Failover mode'da nasil calistirilir?

