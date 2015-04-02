Rabbitmq 
=========


* environment variables define ports, file locations and names (taken from the
shell, or set in the rabbitmq-env.conf file) . Its location is not configurable

* config file(s) : /etc/rabbitmq/rabbitmq.config

Highly Available Queues
~~~~~~~~~~~~~~~~~~~~~~







#. Notlar: 

**Access Control**

* the guest user is prohibited from connecting to the broker remotely; Any
  other users you create will not (by default) be restricted in this way.If you
  wish to allow the guest user to connect from a remote host, you should set
  the loopback_users configuration item to []. A complete rabbitmq.config which
  does this would look like:

.. code::

        [{rabbit, [{loopback_users, []}]}].


* RabbitMQ distinguishes between configure, write and read operations on a
  resource. The configure operations create or destroy resources, or alter
  their behaviour. The write operations inject messages into a resource. And
  the read operations retrieve messages from a resource.RabbitMQ may cache the
  results of access control checks on a per-connection or per-channel basis.
  Hence changes to user permissions may only take effect when the user
  reconnects.

Keepalived
----------




