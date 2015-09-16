Queue 
---

* Kuyrugu izleme
```
postqueue -p
showq - list the Postfix mail queue
```

* Kuyrugu temizleme
```
postqueue -f : Flush the queue: attempt to deliver all queued mail.
FILES /var/spool/postfix, mail queue
postsuper -d ALL
```

* Goruntuleme
```
postcat -vq <queue_id> |less
```

Relay izinleri 
---

* erisim izni olan makinede gerekli paketler kurulur
```
yum install -y cyrus-sasl-plain
```
- Genel ayarlar
```
vi /etc/postfix/main.cf
mynetworks_style = subnet
mynetworks = 127.0.0.0/8, <yerel_ag>/24
inet_interfaces = all
```

- Relay hesabin tanimlanmasi
```
vi /etc/postfix/main.cf
smtp_sasl_mechanism_filter = plain, login
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_always_send_ehlo = yes
relayhost:<ip>

/etc/init.d/postfix restart
echo "<ip>:25    <kullanici_adi>:<parola>" >> /etc/postfix/sasl_passwd
postmap hash:/etc/postfix/sasl_passwd
```

* Yerel sunucudan izinli sunucuya erismek icin
```
vi /etc/postfix/transport
       smtp:<relay_ip>

vi /etc/postfix/main.cf
mynetworks = 127.0.0.0/8, <yerel_ag>/24
relayhost = <relay_ip>
transport_maps =  hash:/etc/postfix/transport

postmap hash:/etc/postfix/transport
```

Sasl ile smtp kullanici dogrulama 
---

```
yum install -y cyrus-sasl cyrus-sasl-plain
/etc/postfix/main.cf
smtpd_sasl_path = sasl2/smtpd.conf
smtpd_sasl_auth_enable = yes
smtpd_sasl_local_domain = <hostname_veya_domain>
smtpd_sasl_security_options = noanonymous
broken_sasl_auth_clients = yes
smtpd_recipient_restrictions = permit_mynetworks,
        permit_sasl_authenticated,
        reject_unauth_destination,
        reject_rbl_client opm.blitzed.org,
        reject_rbl_client list.dsbl.org,
        reject_rbl_client sbl.spamhaus.org,
        reject_rbl_client cbl.abuseat.org,
        reject_rbl_client dul.dnsbl.sorbs.net

/etc/postfix/master.cf
submission inet n       -       n       -       -       smtpd
  -o smtpd_sasl_auth_enable=yes
  -o smtpd_sasl_security_options=noanonymous
  -o smtpd_sasl_local_domain=<hostname_veya_domain>
  -o header_checks=
  -o body_checks=
  -o smtpd_client_restrictions=permit_sasl_authenticated,reject_unauth_destination
  -o smtpd_sasl_security_options=noanonymous,noplaintext
  -o smtpd_sasl_tls_security_options=noanonymous
```

#. sasldb2'de tutulacak kullaniciyi olusturma ve olustugunu gorme;
```
saslpasswd2 -c -u <hostname_veya_domain> <kullanici_adi>
sasldblistusers2
```

* smtpd'nin kullanacagi dogrulama yontemini belirleme;
```
etc/sasl2/smtpd.conf:
    pwcheck_method: auxprop
    auxprop_plugin: sasldb
    mech_list: PLAIN LOGIN CRAM-MD5 DIGEST-MD5
```

* Haklari duzenleme
```
chown postfix: /etc/sasldb2 -R
chown postfix: /etc/postfix -R
chmod 660 /etc/sasldb2
```

* Servisleri yeniden baslatip test etme 

```
/etc/init.d/sasld restart
/etc/init.d/postfix restart
```

* kullanici dogrulamayi test etme

```
python
import smtplib
server = smtplib.SMTP('<sunucu_adresi>', <port_no>)
server.login("kullanici_adi", "parola")
#Send the mail
msg = "\nHello!" 
server.sendmail("<kimden_kisminda_gorulen_adres>", "<gonderilecek_adres>", msg)
```

Kaynaklar
---

* [how-to-rewrite-outgoing-address-in-postfix](http://semi-legitimate.com/blog/item/how-to-rewrite-outgoing-address-in-postfix)
* [Authenticated SMTP with Postfix on CentOS](http://blog.penumbra.be/2010/04/authenticated-smtp-postfix/)
* [Postfix Sasl Okubeni](http://www.postfix.org/SASL_README.html)
* [python smtplib ile mail gonderme](http://www.pythonforbeginners.com/code-snippets-source-code/using-python-to-send-email)


