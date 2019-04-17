Ruby notlari
============


Time Module
-----------

strip module
------------


* oci8 ile oracle veri tabanina erisim
```
/opt/sensu/embedded/bin/ruby -r oci8 -e "OCI8.new('<kullanici_adi>', '<parola>', \
'//<ip>:<port>/<db_adi>').exec('SELECT * FROM <tablo>') do |r| puts r.join(','); end"
```

* gem ile kurulu bir paketi silme: `cleanup`
* spesifik gem versiyonu kurma `--version`

* Should I use `%{}` or `#{}` for formatting?  You will almost always use `#{}` to
  format your strings, but there are times when you want to apply the same
  format to multiple values. That's when `%{}` comes in handy.

#### ESCAPE	WHAT IT DOES.
\\	Backslash ()
\'	Single-quote (')
\"	Double-quote (")
\a	ASCII bell (BEL)
\b	ASCII backspace (BS)
\f	ASCII formfeed (FF)
\n	ASCII linefeed (LF)
\r	ASCII Carriage Return (CR)
\t	ASCII Horizontal Tab (TAB)
\uxxxx	Character with 16-bit hex value xxxx (Unicode only)
\v	ASCII vertical tab (VT)
\ooo	Character with octal value ooo
\xhh	Character with hex value hh

[Kaynak:learnrubythehardway](http://learnrubythehardway.org/book/ex10.html)

* `ruby dosya_adi.rb` calistiridiginda, ikinci kisma arguman deniyor.

* `howdoi ruby open` ciktisi soyle;

Mode |  Meaning
-----+--------------------------------------------------------
"r"  |  Read-only, starts at beginning of file  (default mode).
-----+--------------------------------------------------------
"r+" |  Read-write, starts at beginning of file.
-----+--------------------------------------------------------
"w"  |  Write-only, truncates existing file
     |  to zero length or creates a new file for writing.
-----+--------------------------------------------------------
"w+" |  Read-write, truncates existing file to zero length
     |  or creates a new file for reading and writing.
-----+--------------------------------------------------------
"a"  |  Write-only, starts at end of file if file exists,
     |  otherwise creates a new file for writing.
-----+--------------------------------------------------------
"a+" |  Read-write, starts at end of file if file exists,
     |  otherwise creates a new file for reading and
     |  writing.
-----+--------------------------------------------------------
"b"  |  Binary file mode (may appear with
     |  any of the key letters listed above).
     |  Suppresses EOL <-> CRLF conversion on Windows. And
     |  sets external encoding to ASCII-8BIT unless explicitly
     |  specified.
-----+--------------------------------------------------------
"t"  |  Text file mode (may appear with
     |  any of the key letters listed above except "b").


* actigin bir dosyayi kapatman da onemli isin bittiginde close()'u da kullan.

* `gets` is the Ruby method that gets input from the user. When getting input,
  Ruby automatically adds a blank line (or newline) after each bit of input;
  chomp removes that extra line. (Your program will work fine without chomp, but
  you'll get extra blank lines everywhere.)

* Functions do three things:

  - They name pieces of code the way variables name strings and numbers.
  - They take arguments the way your scripts take ARGV.
  - Using 1 and 2 they let you make your own "mini-scripts" or "tiny commands."


* `return` set variables to be a value from a function

* `+`  plus
* `-`  minus
* `/`  slash
* `*`  asterisk
* `%`  percent
* `<`  less-than
* `>`  greater-than
* `<=` less-than-equal
* `>=` greater-than-equal


