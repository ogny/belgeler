Queue 
~~~~~

#. Kuyrugu izleme::

    postqueue -p
    showq - list the Postfix mail queue

#. Kuyrugu temizleme::

    postqueue -f : Flush the queue: attempt to deliver all queued mail.
    FILES /var/spool/postfix, mail queue

#. Goruntuleme::

    postcat -vq <queue_id> |less

#. Yonetim::

    postsuper -d ALL

#. Calisilacak::

    Relay server 



