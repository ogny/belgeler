Centos 6.6 kurulum
~~~~~~~~~~~~~~~~~~

::

    curl -L -O https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py
    python ez_setup.py
    easy_install pip

#. pip guncellemelerinde olusan hatalarin duzeltilmesi;
  
   `InsecurePlatformWarning`
..code:: sh

    pip install requests[security]

   `Pip untrusted host`

[global]
index-url = http://osrepo.xxx.in/pypi/simple
trusted-host = osrepo.xxx.in
disable-pip-version-check = true 
allow-all-external=true
timeout = 120
    
#. pip dependency'leriyle paket kaldirma

#. install pip-autoremove
`pip install pip-autoremove`
# remove "somepackage" plus its dependencies:
`pip-autoremove somepackage -y`


