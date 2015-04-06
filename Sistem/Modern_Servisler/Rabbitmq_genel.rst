Rabbitmq  3.5.1 HA Cluster
==========================

#. Kurulum::

   rpm --import http://www.rabbitmq.com/rabbitmq-signing-key-public.asc
   yum install -y erlang 
   curl -L -O http://www.rabbitmq.com/releases/rabbitmq-server/current/rabbitmq-server-3.5.1-1.noarch.rpm
   yum install -y rabbitmq-server-3.5.1-1.noarch.rpm
   chkconfig rabbitmq-server on

#. Sistem Yapilandirma::

   ulimit -n 63536
   echo "fs.file-max = 65536" | tee -a /etc/sysctl.conf && sysctl -p
   vi /etc/security/limits.conf
   *    soft    nofile          63536
   *    hard    nofile          63536
   vi /etc/security/limits.d/90-nproc.conf
   *    soft    nproc           63536
    
#. Cluster'a dahil edilecek makinalar hosts dosyasina eklenir.
#. Master node'tan erlang.cookie digerlerine transfer edilir::  

    scp /var/lib/rabbitmq/.erlang.cookie \
    root@<node_hostname>:/var/lib/rabbitmq/

#. Rabbitmq tum node'larda baslatilir::

    rabbitmq-server -detached 

#. Cluster olusturma (Master-slave1-slave2)

    - Slave'lerde teker teker uygulanir::

    rabbitmqctl stop_app
    rabbitmqctl join_cluster rabbit@master
    rabbitmqctl start_app
    rabbitmqctl cluster_status

#. Set the HA Policy: The following command will sync all the queues across all
   nodes::

   rabbitmqctl set_policy ha-all "" \
   '{"ha-mode":"all","ha-sync-mode":"automatic"}'



#. Web Management Console'u baslatma ve erisim (master'da)::

    rabbitmq-plugins enable rabbitmq_management
    http://<ip_adresi>:15672    

    rabbitmqctl add_user test test
    rabbitmqctl set_user_tags test administrator
    rabbitmqctl set_permissions -p / test ".*" ".*" ".*"

#. Web Management Console'da Slave'leri izleyebilmek icin(slave'lerde)::

    rabbitmq-plugins enable rabbitmq_management_agent

#. Kullanim; baslatma, durdurma::

    rabbitmq-server -detached
    rabbitmqctl stop
        This command instructs the RabbitMQ node to terminate.
    rabbitmqctl stop_app
        This command instructs the RabbitMQ node to stop the RabbitMQ
        application.
    rabbitmqctl start_app
        This command instructs the RabbitMQ node to start the RabbitMQ
        application.
    rabbitmqctl force_boot
         This will force the node not to wait for other nodes next time it
         is started.


#. Guvenlik, Acilacak portlar::

    4369 (epmd), 25672 (Erlang distribution)
    5672, 5671 (AMQP 0-9-1 without and with TLS)
    15672 (if management plugin is enabled)
    61613, 61614 (if STOMP is enabled)
    1883, 8883 (if MQTT is enabled)
    

Cases
-----

#. Butun cluster cokerse son coken node master yapilmali, cunku en guncel olan
   o; When the entire cluster is brought down, the last node to go down must be
   the first node to be brought online. (fifo)

NOT
~~~

#. Cluster Yonetimi::

    After executing the cluster command, whenever the RabbitMQ application is
    started on the current node it will attempt to connect to the nodes that
    were in the cluster when the node went down.

    To leave a cluster, reset the node. You can also remove nodes
    remotely with the forget_cluster_node  command.

    cluster_status Displays all the nodes in the cluster grouped by node type

    update_cluster_nodes (daha once cluster'a dahil olan bir node'un dustugu
    durumda yeni cluster'a tekrar sokmak icin)


    set_cluster_name {name}  herhangi bir atama yapilmadiginda cluster'in adi
    ilk node'un hostname'inden turetilir, ancak bu komutla degistirilebilir.
    federation and shovel plugin'leri mesajlari kaydetmek icin bilirler.

#. Kullanici Yonetimi::

    rabbitmqctl RabbitMQ'nun internal db'sini yonetiyor, baska yontemlerle
    olusturulan kullanicilardan habersizdir.

#. Parametre Yonetimi::

    Each parameter consists of a component name, a name and a value, and is
    associated with a virtual host. The component name and name are strings, and
    the value is an Erlang term.

#. Policy Management:: 

    Policies are used to control and modify the behaviour of
    queues and exchanges on a cluster-wide basis. Policies apply within a given
    vhost, and consist of a name, pattern, definition and an optional priority.

 #. Server Status komutlari::

    list_queues, list_exchanges, list_bindings, list_connections

#. Miscellaneous::

    close_connection 
    trace_on

 #. Servis baslatma::

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

    /etc/rabbitmq/rabbitmq-env.config

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

#. Queues have mirroring enabled via policy.

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




