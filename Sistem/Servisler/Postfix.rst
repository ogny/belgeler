Queue 
~~~~~

#. Kontrol::


    postqueue -f : Flush the queue: attempt to deliver all queued mail.
    FILES /var/spool/postfix, mail queue
    showq - list the Postfix mail queue

#. Goruntuleme::

    postcat -vq <queue_id> |less

#. Yonetim::

    postsuper -d ALL

#. Calisilacak::

    Relay server 



