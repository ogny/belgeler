### Best Practises
 
* Resource'lar isin temeli; kullanabilecegin, olusturulmus kaynaklar- 
  recipe'ler bunlara gore yaziliyor.

* **Resources have a type.** The LWRP’s **resource type** is defined by the
  name of the file within the cookbook. This implicit name follows the formula
  of: **cookbook_resource**. If the default.rb file is used the new resource
  will be named cookbook.

* When you start writing your first roles, you tend to define them as a
  run-list with a collection of attributes. Mostly because that’s the
  documentation about, but it’s not necessary a good practice, depending very
  much of your business needs. Roles in chef are un-versioned. But cookbooks
  are versioned… Role Cookbooks! (ipam projesinde bu sekilde kullaniyoruz)

* Using this approach still allows us to have a single role web, shared across
  the environments. However, the cookbook associated with this role can have as
  many versions as we need, controlling the evolution of the role and the
  promotion of the role between different environments. And all this by
  changing the role’s cookbook constrained in the target environment.





test cookbook'u olusturdugunda dizin hiyerarsisi;
``` 
attributes 
CHANGELOG.md 
definitions 
files 
libraries 
metadata.rb 
providers 
README.md 
recipes 
resources 
templates 
``` 

* Chef gives us the ability to extend functionality with Definitions, Heavy
  Weight Resources and Providers (HWRP), and Light Weight Resources and
  Providers (LWRP).

* test icin cookbook yaratma;
```
knife cookbook create test_coobook -o .
```

============
chef calisma
============

#. Chef is a system integrations framework built on top of a configuration
   management system. It’s powered by Ruby and by a simple DSL.

#. chef-apply ile her yaptigin islem icin git snapshot'indaki gibi bir UUID
   donduruyor. her islem incremental olarak tutuluyor. Ancak bir onceki
   commit'in icerigi yeni commit yapildiginda siliniyor. Soru: Bu durumdan
   kurtulmak icin varolan bir dosyaya append nasil edilir?

#. ruby dosyasindan  dosya olusturabilecegin gibi bash komutlariyla da
   olusturabiliyorsun.

#. Guzel bir tanimlama; "Resources describe the what, not the how"

#. Our recipe states everything we need to manage the MOTD file. 

#. Action'larda bir detay; 
   Resource'lara gore action'lar tanimli, ornegin;
   #. file resource'unda default action create
   #. package resource'unda default action install
   Buna gore, biz tanimlari olustururken once neye gore (hangi resource'a gore)
   olusturdugumuzu belirtiyoruz.

#. Generate ettiginde chef'in olusturdugu default dosya agaci;

.
└── learn_chef_httpd
    ├── Berksfile
    ├── chefignore
    ├── metadata.rb
    ├── README.md
    └── recipes
        └── default.rb

```
   chef-client --local-mode --runlist 'recipe[learn_chef_httpd]'
```

#. The run-list lets you specify which recipes to run, and the order in which
   to run them

#. A typical Chef workflow consists of three parts – your workstation, a Chef
   server, and nodes.

#. knife enables you to communicate with the Chef server and download shared
   community cookbooks.

#. workstation'a chefdk'yi kurup knife'i ilk calistirdigimda olusan uyari;


```
   WARNING: No knife configuration file found
```

baska bir makinadan key'leri ve knife.rb'yi alip knife'i kullanmaya
baslayabilirsin.


Tanimlar;
=========

Chef Bilesenleri;
-----------------

#. chef-client; is installed on every node that is under management by Chef.

#. The Chef server acts as a hub of information. Cookbooks and policy settings
   are uploaded to the Chef server by users from workstations. (Policy settings
   may also be maintained from the Chef server itself, via the Chef management
   console web user interface.) The chef-client accesses the Chef server from
   the node on which it’s installed to get configuration data, perform searches
   of historical chef-client run data, and then pulls down the necessary
   configuration data. After the chef-client run is finished, the chef-client
   uploads updated chef-client run data to the node object and generates
   reporting data about that chef-client run.

#. node: any physical, virtual, or cloud machine that is configured to be
   maintained by a chef-client. ( network cihazlari da birer node olarak
   gorulebilir. Dokumana gore Juniper, Arista, Cisco, ve F5 markalarinin
   cihazlari yonetilebilir.
   
Node'lara kurulan bilesenler;
-----------------------------

#. chef-client; is an agent that runs locally on every node that is under
   management by Chef. 

#. Ohai is a tool that is used to detect attributes on a node, and then provide
   these attributes to the chef-client at the start of every chef-client run

   The types of attributes Ohai collects include (but are not limited to):
   
   Platform details
   Network usage
   Memory usage
   CPU data
   Kernel data
   Host names
   Fully qualified domain names
   Other configuration details

Workstation
-----------

#. A workstation is a computer that is configured to run knife, to synchronize
   with the chef-repo, and interact with a single Chef server. 

#. Chef incudes two important command-line tools. Use the knife command-line
   tool when interacting with nodes or when working with objects on the Chef
   server. Use the chef command line tool when working with the chef-repo,
   which is the repository structure in which cookbooks are authored, tested,
   and maintained.

#. Policy defines how business and operational requirements, processes, and
   production workflows map to objects that are stored on the Chef server.
   Policy objects on the Chef server include roles, environments, and cookbook
   versions.

Chef Server
-----------

#. Nodes use the chef-client to ask the Chef server for configuration details,
   such as recipes, templates, and file distributions. The chef-client then
   does as much of the configuration work as possible on the nodes themselves
   (and not on the Chef server). 

Node Objects
------------

#.  The node object consists of the run-list and node attributes, which is a
    JSON file that is stored on the Chef server. The chef-client gets a copy of
    the node object from the Chef server during each chef-client run and places
    an updated copy on the Chef server at the end of each chef-client run.

Calisma Prensibi
----------------

#. When a node and/or a workstation is configured to run the chef-client, both
   public and private keys are created. The public key is stored on the Chef
   server, while the private key is returned to the user for safe keeping. (The
   private key is a .pem file located in the .chef directory or in /etc/chef.)

#. Knife ve chef-client birlikte islenmeli, calisma prensipleri ayni, pubkey
   ile chef server'a authenticate oluyorlar.

Get Specific Nodes
------------------

#. To get a list of nodes using a recipe named postfix use
   search(:node,"recipe:postfix"). To get a list of nodes using a sub-recipe
   named delivery, use chef-shell. For example:

```
    search(:node, 'recipes:postfix\:\:delivery')
```

#. Note Single (‘ ‘) vs. double (” ”) is important. This is because a backslash
   () needs to be included in the string, instead of having Ruby interpret it
   as an escape.

#. chef-solo is an open source and limited version of the chef-client. 

Knife
-----

#. an interface between a local chef-repo and the Chef server. knife helps
   users to manage:

   #. Nodes
   #. Cookbooks and recipes
   #. Roles
   #. Stores of JSON data (data bags), including encrypted data
   #. Environments
   #. Cloud resources, including provisioning
   #. The installation of the chef-client on management workstations
   #. Searching of indexed data on the Chef server

#. Sub-commands

bootstrap, client, configure, cookbook, cookbook site, data bag, delete, deps,
diff, download, edit, environment, exec, index rebuild, list, node, recipe
list, role, search, show, ssh, status, tag, upload, user, and xargs.

#. Yeni bir Environment olusturmanin onemi;

For instance, one environment may be called "testing" and another may be called
"production". Since you don't want any code that is still in testing on your
production machines, each machine can only be in one environment. You can then
have one configuration for machines in your testing environment, and a
completely different configuration for computers in production.

#. Role olustururken  "env_run_lists": degeri ile prod'la test'i ayirabiliyoruz

#. Cookbook'lari test etmek icin;

```
   knife cookbook test COOKBOOK_NAME (options) 
```


Data Bags
-------------------------

#. A data bag is a global variable that is stored as JSON data and is
   accessible from a Chef server. A data bag is indexed for searching and can
   be loaded by a recipe or accessed during a search.  **data_bags usage**; for
   storing data that doesn’t map one-to-one with nodes.  For example, having a
   bag calls users with one item for each of your system users with keys like
   name and ssh_keys. You can then write a single Chef recipe to loop over each
   item in the bag and create their user account, populate authorized_keys, and
   so on.

Providers
---------

#. Generally, it’s best to let the chef-client choose the provider and this is
   (by far) the most common approach.

Community Handlers (kullanacaklarimiz)
-------------------------

#. Graphite HipChat Graylog2 Journald Syslog


About Configuration Files
-------------------------

#. chef-server.rb Settings: A chef-server.rb file is used to specify the
   configuration settings for the Chef server.

#. client.rb: A client.rb file is used to specify the configuration details for
   the chef-client.

#. knife.rb: A knife.rb file is used to specify the chef-repo-specific
   configuration details for knife.

About Definitions
-----------------

#. A definition is code that is reused across recipes

    #. Is defined from within the /definitions directory of a cookbook
    #. Is loaded before resources during the chef-client run; this ensures the
       definition is available to all of the resources that may need it
    #. May not notify resources in the resource collection because a definition
       is loaded before the resource collection itself is created; however,
       a resource in a definition may notify a resource that exists within
       the same definition

Syntax
------

A definition has four components:

#. A resource name
#. Zero or more arguments that define parameters their default values; if a
   default value is not specified, it is assumed to be nil
#. A hash that can be used within a definition’s body to provide access to
   parameters and their values
#. The body of the definition
#. The basic syntax of a definition is:

```
    define :resource_name do
      body
    end
```

#. An example showing the more common usage pattern, a definition named
   apache_site with an parameter called action with an argument for enable,
   would look something like:

```
    define :apache_site, :action => :enable do
      if params[:action] == :enable
         ...
      else
         ...
      end
    end
```

#. Once created, a definition is used just like a resource.


About Resources
----------------

#. Each resource statement in a Chef recipe corresponds to a specific part of
   your infrastructure: a file, a template, a directory, a package, a service,
   a command to be executed, and so on. Each resource statement includes the
   resource type (such as template, service or package), its name, any
   attributes that specify additional details, and an action that tells the
   chef-client how to implement the configuration policy.

#. Where a resource represents a piece of the system (and its desired state), a provider defines the steps that are needed to bring that piece of the system from its current state into the desired state.

Templates:
----------

#. erb dosyalariyla calisma


BERKSHELF
----------

Kaynaklar
--------

* [resource olusturma](thttp://sysadvent.blogspot.com.tr/2014/12/day-21-baking-delicious-resources-with.html)
* [cookbook olusturma](http://silviud.blogspot.com.tr/2014/03/vim-setup-for-chefopscode-cookbooks.html)
* [cookbook patterns best practices](http://bytearrays.com/chef-cookbook-patterns/)
* [attributes or data bags what should i use](https://www.chef.io/blog/2014/01/23/attributes-or-data-bags-what-should-i-use/)
* [Email listesinin RSS'i;](http://lists.opscode.com/sympa/rss/latest_arc/chef?count=20&for=10)
* [bootstrap:](https://learn.chef.io/rhel/bootstrap-your-node/)
* [BERKSHELF;](<http://berkshelf.com/)
* [learning chef](http://nathenharvey.com/blog/2012/12/06/learning-chef-part-1/)

