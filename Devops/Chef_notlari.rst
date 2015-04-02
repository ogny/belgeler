====
Chef
====

#. Server Kuruluma baslandi devam edilecek::

    curl -L -O  https://web-dl.packagecloud.io/chef/stable/packages/el/6/chef-server-core-12.0.7-1.el6.x86_64.rpm
    rpm -Uvh chef-server-core*.rpm
# warning: chef-server-core-12.0.7-1.el6.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 83ef826a: NOKEY::

    chef-server-ctl reconfigure
    chef-server-ctl user-create <eklenecek> 
    chef-server-ctl org-create <eklenecek> 
    chef-server-ctl org-create <eklenecek> 
    wget https://packagecloud.io/chef/stable/el/6/x86_64/repodata/repomd.xml
    vi /etc/yum.repos.d/chef-stable.repo
        enabled=1
    chef-server-ctl install opscode-manage
    chef-server-ctl org-list
    vi /opt/opscode/chef-server-plugin.rb
    vi /opt/opscode/embedded/cookbooks/yum/providers/repository.rb
    vi /opt/opscode/embedded/cookbooks/yum/templates/default/repo.erb
    vi /opt/opscode/embedded/cookbooks/yum/attributes/main.rb
    vi /opt/opscode/embedded/cookbooks/yum/templates/default/repo.erb

#. `kaynak <https://docs.chef.io/install_server.html>`_
