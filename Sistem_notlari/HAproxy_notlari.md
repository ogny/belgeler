#### HAProxy is a TCP/HTTP reverse proxy 

* route HTTP requests depending on statically assigned cookies
* persistence through the use of HTTP cookies
* switch to backup servers in the event a main one fails
* stop accepting connections without breaking existing ones
* hold clients to the right application server depending on 
* -m <megs>: Enforce a memory usage limit to a maximum of <megs> megabytes


url'lerin yonlendirilmesi
* reqrep  <search> <string>
* reqirep <search> <string> [{if | unless} <cond>]   (ignore case)
Replace a regular expression with a string in an HTTP request line
** replace "/static/" with "/" at the beginning of any request path.
reqrep ^([^\ :]*)\ /static/(.*)     \1\ /\2
** replace "www.mydomain.com" with "www" in the host name.
reqirep ^Host:\ www.mydomain.com   Host:\ www

*rspirep <search> <string> [{if | unless} <cond>]  (ignore case)
Replace a regular expression with a string in an HTTP response line




  Arguments :
    <search>  is the regular expression applied to HTTP headers and to the
              request line. This is an extended regular expression. Parenthesis
              grouping is supported and no preliminary backslash is required.
              Any space or known delimiter must be escaped using a backslash
              ('\'). The pattern applies to a full line at a time. The "reqrep"
              keyword strictly matches case while "reqirep" ignores case.

    <string>  is the complete line to be added. Any space or known delimiter
              must be escaped using a backslash ('\'). References to matched
              pattern groups are possible using the common \N form, with N
              being a single digit between 0 and 9. Please refer to section
              6 about HTTP header manipulation for more information.



The following ACL flags are currently supported :

   -i : ignore case during matching of all subsequent patterns.
   -f : load patterns from a file.
   -m : use a specific pattern matching method
   -n : forbid the DNS resolutions
   -M : load the file pointed by -f like a map file.
   -u : force the unique id of the ACL
   -- : FORCe end of flags. Useful when a string looks like one of the flags.


The "-m" flag is used to select a specific pattern matching method on the input
sample. All ACL-specific criteria imply a pattern matching method and generally
do not need this flag. However, this flag is useful with generic sample fetch
methods to describe how they're going to be matched against the patterns.


 The pattern matching method must be one of the following :

- "found"
- "bool"
- "int"
- "ip"
- "bin"
- "len"
- "str"
- "sub"
- "reg"
- "beg"
- "end"
- "dir"
- "dom"

[Kaynak:](http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#7)

In order to apply a regular expression on the 500 first bytes of data in the
buffer, one would use the following acl :

    acl script_tag payload(0,500) -m reg -i <script>

On systems where the regex library is much slower when using "-i", it is
possible to convert the sample to lowercase before matching, like this :

    acl script_tag payload(0,500),lower -m reg <script>

7.1.3. Matching strings
String matching applies to string or binary fetch methods, and exists in 6
different forms :

  - exact match     (-m str) : the extracted string must exactly match the
    patterns ;
  - substring match (-m sub) : the patterns are looked up inside the
    extracted string, and the ACL matches if any of them is found inside ;
  - prefix match    (-m beg) : the patterns are compared with the beginning of
    the extracted string, and the ACL matches if any of them matches.
  - suffix match    (-m end) : the patterns are compared with the end of the
    extracted string, and the ACL matches if any of them matches.
  - subdir match    (-m sub) : the patterns are looked up inside the extracted
    string, delimited with slashes ("/"), and the ACL matches if any of them
    matches.
  - domain match    (-m dom) : the patterns are looked up inside the extracted
    string, delimited with dots ("."), and the ACL matches if any of them
    matches.

It is also possible to form rules using "anonymous ACLs". Those are unnamed ACL
expressions that are built on the fly without needing to be declared. They must
be enclosed between braces, with a space before and after each brace

nbsrv([<backend>]) : integer
Returns an integer value corresponding to the number of usable servers of
either the current backend or the named backend. This is mostly used with
ACLs but can also be useful when added to logs. This is normally used to
switch to an alternate backend when the number of servers is too low to
to handle some load. It is useful to report a failure when combined with
"monitor fail".

* Hata: HAProxy “503 Service Unavailable” for webserver   
Cozum: Alright, I don't know what has happened but I changed this line option httpchk
HEAD /check.txt HTTP/1.0 for this one option httpchk HEAD / HTTP/1.0 and start
the server after starting HAProxy. Then it works as expected.
[Kaynak:](http://serverfault.com/questions/433936/haproxy-503-service-unavailable-for-webserver-running-on-a-kvm-virtual-machine)
