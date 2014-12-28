server ve client icin; syslog-ng paketi kurulumda rsyslog paketini kaldiriyor

### Tanimlar

* A log path consists of one or more sources and one or more
destinations; messages arriving from a source are sent to every destination
listed in the log path. A log path defined in syslog-ng is called a log
statement.

* 2.8.1.1. The PRI message part
The PRI part of the syslog message (known as Priority value) represents the
Facility and Severity of the message. Facility represents the part of the
system sending the message, while severity marks its importance. The Priority
value is calculated by first multiplying the Facility number by 8 an then
adding the numerical value of the Severity.

* IETF-syslog protocol). A syslog message consists of the following parts:
■ HEADER (includes the PRI as well) ■ STRUCTURED-DATA ■ MSG ** The message
corresponds to the following format: ``` <priority>VERSION ISOTIMESTAMP
HOSTNAME APPLICATION PID MESSAGEID STRUCTURED-DATA MSG ```

** The STRUCTURED-DATA message part STRUCTURED-DATA consists of data blocks
enclosed in brackets ([]). Every block includes the ID of the block, and one
or more name=value pairs.

* When using value-pairs, there are three ways to specify which information
(that is, macros or other name-value pairs) to include in the selection.
■ Select groups of macros using the scope() parameter, and optionally remove
certain macros from the group using the exclude() parameter.
■ List specific macros to include using the key() parameter.

* The syslog-ng application allows you to specify the data type in templates 
(this is also called type-hinting). If the destination driver supports data types, 
it converts the incoming data to the specified data type. 

** To use type-hinting, enclose the macro or template containing the data with the
type: <datatype>("<macro>") , for example: int("$PID") .  Currently the
mongodb() destination and the format-json template function supports data
types.  Example 2.1. Using type-hinting The following example stores the
MESSAGE, PID, DATE, and PROGRAM fields of a log message in a MongoDB database.

* The value-pairs() option has the following parameters. The parameters are
evaluated in the following order:

1. scope()
■ all-nv-pairs: Include every soft macro (name-value pair). Equivalent to using
both nv-pairs and
■ selected-macros: Include the macros of the rfc3164 and rfc5424 groups, and
the most commonly used metadata about the log message: the $TAGS , $SOURCEIP ,
and $SEQNUM macros.  
■ sdata: The metadata from the structured-data (SDATA) part of
RFC5424-formatted messages, that is, every macro that starts with .SDATA.

2. exclude()
3. key()
4. pair()
exclude()

Type:
Space-separated list of macros to

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
* testing command: loggen
--inet or -i
Use the TCP (by default) or UDP (when used together with the --dgram
option) protocol to send the messages to the target
```
loggen -i localhost
```

### Sorular-Inceleme;
* output'lari nasil verebiliriz? format-json()  template fonksiyonunu incele, 



