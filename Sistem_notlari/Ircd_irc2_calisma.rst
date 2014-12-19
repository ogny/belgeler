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
