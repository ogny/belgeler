#### Kurulum
```
wget https://97.107.139.216/linode.gpg
apt-key add linode.gpg && \
apt-get update && \
apt-get install linode-cli && \
```

#### Kullanim
* linode configure ile temel yapilandirma.
* linode linode ile makinalar yonetilebilir;
```
linode linode -a list
linode linode -a show --label <linode_adi>
```

