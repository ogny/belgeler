#### Kurulum
```
wget --no-check-certificate https://97.107.139.216/linode.gpg
echo "deb http://apt.linode.com/ stable main" \
> /etc/apt/sources.list.d/linode.list
apt-key add linode.gpg && \
apt-get update && \
apt-get install linode-cli
```

#### Kullanim
* linode configure ile temel yapilandirma.
* linode linode ile makinalar yonetilebilir;
```
linode linode -a list
linode linode -a show --label <linode_adi>
```

