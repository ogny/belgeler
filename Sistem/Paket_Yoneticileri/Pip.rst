Centos 6.6 kurulum
~~~~~~~~~~~~~~~~~~

#.  Pip kurulum (ex_setup'tan daha guncel)::

    curl -L -O https://bootstrap.pypa.io/get-pip.py
    python get-pip.py

    curl -L -O https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py
    python ez_setup.py
    easy_install pip

#. pip guncellemelerinde olusan hatalarin duzeltilmesi;
  
   `InsecurePlatformWarning`
..code:: sh

   pip install requests[security]
   Pip untrusted host

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

#. `pip search` ile gordugun bir paketi `install` ile kuramadiginda `-vvv` ile hatayi gor::

  DistributionNotFound: No matching distribution found for <paket_adi>
  pip buraya bakiyor; http://pypi.python.org/simple/<paket_adi>

#. whl paketi indirme, kurma::

   pip install meld3 --download  dizin
   pip install --user dizin/meld3-1.0.2-py2.py3-none-any.whl


   #. sudo izni olmayan kullanici ile kurulum, guncelleme::

   pip install --user paket_adi
   pip install --user --upgrade pip

#. pip 8.0.2'de download default geliyor::

   cd indirilecek_dizin
   pip download supervisor

#. pip olmayan ortamda pip kurulumu::

    python pip-8.0.2-py2.py3-none-any.whl/pip install \
    --no-index pip-8.0.2-py2.py3-none-any.whl

#. pip'le kurarken flag vermek mumkun::

    pip install <paket> --global-option="build_ext" --global-option="--disable-<bagimlilik>"

