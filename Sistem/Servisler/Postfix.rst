Queue 
~~~~~

#. Kuyrugu izleme::

    postqueue -p
    showq - list the Postfix mail queue

#. Kuyrugu temizleme::

    postqueue -f : Flush the queue: attempt to deliver all queued mail.
    FILES /var/spool/postfix, mail queue
    postsuper -d ALL

#. Goruntuleme::

    postcat -vq <queue_id> |less

Relay izinleri 
~~~~~~~~~~~~~~~


#. erisim izni olan makinede 

- Gerekli paketler kurulur::

    yum install -y cyrus-sasl-plain

- Genewl ayarlar::

    vi /etc/postfix/main.cf
    mynetworks_style = subnet
    mynetworks = 127.0.0.0/8, <yerel_ag>/24
    inet_interfaces = all

    vi /etc/postfix/master.cf
    0.0.0.0:smtp      inet  n       -       n       -       -       smtpd

- Relay hesabin tanimlanmasi::

    vi /etc/postfix/main.cf
    smtp_sasl_auth_enable = yes
    smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
    smtp_sasl_security_options = noanonymous
    smtp_always_send_ehlo = yes
    relayhost:<ip>:25

    /etc/init.d/postfix restart
    echo "<ip>:25    ipamraporlama@turktelekom.com.tr:Telekom123" >>
    /etc/postfix/sasl_passwd
    postmap hash:/etc/postfix/sasl_passwd

#. Yerel sunucudan izinli sunucuya erismek icin::

   vi /etc/postfix/transport
   *       smtp:<relay_ip>

    vi /etc/postfix/main.cf
    mynetworks = 127.0.0.0/8, <yerel_ag>/24
    relayhost = <relay_ip>
    transport_maps =  hash:/etc/postfix/transport

    postmap hash:/etc/postfix/transport

#. Dikkat edilecek nokta:: 
   /etc/postfix altinda sasl_passwd disindaki dosyalarin haklari postfix'in
   olsun

