* istedigin sayida kullanicinin login olabilmesi icin;


**New (as of 2.11) format of users and rejects logfiles.**

* Verbatim from the common/send.c:

.. code:: c

        (void)sprintf(linebuf,
        "%c %d %d %s %s %s %s %d %s %lu %llu %lu %llu",
        /* exit code as defined in common/struct_def.h; some common:
         * '0' normal exit, '-' unregistered client quit, 'k' k-lined,
         * 'K' killed, 'X' x-lined, 'Y' max clients limit of Y-line,
         * 'L' local @host limit, 'l' local user@host limit, 'P' ping
         * timeout, 'Q' send queue exceeded, 'E' socket error */
        cptr->exitc,
        /* signon unix time */
        (u_int) cptr->firsttime,
        /* signoff unix time */
        (u_int) timeofday,
        /* username (if ident is not working, it's from USER cmd) */
        username,
        /* hmm, let me take an educated guess... a hostname? */
        hostname,
        /* ident, if available */
        cptr->auth ? cptr->auth : "",
        /* client IP */
        cptr->user ? cptr->user->sip :
        #ifdef INET6
        inetntop(AF_INET6, (char *)&cptr->ip, mydummy, MYDUMMY_SIZE),
        #else
        inetntoa((char *)&cptr->ip),
        #endif
        /* client (remote) port */
        cptr->port,
        /* server sockhost (IP plus port or unix socket path) */
        cptr->acpt ? cptr->acpt->sockhost : "?",
        /* messages and bytes sent to client */
        cptr->sendM, cptr->sendB,
        /* messages and bytes received from client */
        cptr->receiveM, cptr->receiveB);


`Kaynak:42.pl <http://42.pl/ircd/logformat.html>`_

The ircd.conf file contains various records that specify configuration
options. The record types are as follows:

  1. Machine information (M)
  2. Administrative info (A)
  3. Port connections (P)
  4. Connection Classes (Y)
  5. Client connections (I,i)
  6. Operator privileges (O)
  7. Restrict lines (R)
  8. Excluded accounts (K,k)
  9. Server connections (C,c,N)
  10. Deny auto-connections (D)
  11. Hub connections (H)
  12. Leaf connections (L)
  13. Version limitations (V)
  14. Excluded machines (Q)
  15. Service connections (S)
  16. Bounce server (B)
  17. Default local server (U)

* Except for types 'M' and 'A', you are allowed to have multiple records of the
same type. In some cases, you can have concurrent records.  
It is important to note that the last matching record will be used.
This is especially useful when setting up I records (client connections).

* Port connections: coklu sunucuyla calismak icin yapilandirma bu baslikta.




* Sunucu 

    Y:<Class>:<Ping Frequency>:<Connect freq>:<Max Links>:<SendQ>:<Local Limit>:<Global Limit>

Sunucuya internetten erisecek kullanicilari sinirlandirirken local kullanici
sinirlandirmasina ihtiyac var, aksi durumda global limit gecerli olmuyor.

* Istemci 

   I:<TARGET Host Addr>:<Password>:<TARGET Hosts NAME>:<Port>:<Class> 

