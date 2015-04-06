Rabbitmq 
=========


#. Kurulum::

   rpm --import http://www.rabbitmq.com/rabbitmq-signing-key-public.asc
   yum install -y erlang 
   curl -L -O http://www.rabbitmq.com/releases/rabbitmq-server/current/rabbitmq-server-3.5.1-1.noarch.rpm
   yum install -y rabbitmq-server-3.5.1-1.noarch.rpm
   chkconfig rabbitmq-server on

#. Cluster'a dahil edilecek makinalar hosts dosyasina eklenir.
#. Web Management Console'u baslatma ve erisim::

    rabbitmq-plugins enable rabbitmq_management
    http://<ip_adresi>:15672    

#.  Baslatma::


    rabbitmq-server -detached


HA Failover Cluster (master-master)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~




NOT
~~~

Cluster Yonetimi::

    After executing the cluster command, whenever the RabbitMQ application is
    started on the current node it will attempt to connect to the nodes that
    were in the cluster when the node went down.

To leave a cluster, reset the node. You can also remove nodes
remotely with the `forget_cluster_node` command.


#. Kullanima dair::

    start_app stops the Rabbit application inside the Erlang VM but the VM has
    to be running for that to work.
    stop_app will stop the rabbit app, but not the Erlang VM
    rabbitmq-server takes care of preparing the env parameters and starting the
    Erlang VM to then run RabbitMQ on it.

    RABBITMQ_NODE_PORT 5672

    reset'lemek icin once durdurmus olman lazim (e.g. with stop_app)

#. Servis olarak yonetme:: 

    "-detached" tells Erlang to fork the process. 
    "&" tells the shell to fork the process. 
    The only difference is that you get a PID file with "&", which then lets 
    you use "rabbitmqctl wait" to make sure you know when the server has 
    finished starting. That's what the init.s scripts for our .deb and RPM 
    packages do. 
    Start the server process in the background. Note that this will cause the pid
    not to be written to the pid file.

#. environment variables define ports, file locations and names (taken from the
   shell, or set in the rabbitmq-env.conf file) . Its location is not
   configurable::

    /etc/rabbitmq/rabbitmq.config




Teori
-----

#. RabbitMQ supports clustering by default, but queues aren't replicated and
   are bound to the node on which they're created.

#. Queues have mirroring enabled via policy. Policies can change at any time;
   it is valid to create a non-mirrored queue, and then make it mirrored at
   some later point (and vice versa). There is a difference between a
   non-mirrored queue and a mirrored queue which does not have any slaves 

#. You could use an active/passive pair of nodes such that should one node
   fail, the passive node will be able to come up and take over from the failed
   node. This can even be combined with clustering. 




Queues have mirroring enabled via policy.

Note that setting or modifying a "nodes" policy can cause the existing master
to go away if it is not listed in the new policy.



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




