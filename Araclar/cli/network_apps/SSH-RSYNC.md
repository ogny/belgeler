Set up trusted copy between postgres accounts
---------------------------------------------

If you need to use `rsync` to clone standby servers, the `postgres` account
on your primary and standby servers must be each able to access the other
using SSH without a password.

First generate an ssh key, using an empty passphrase, and copy the resulting
keys and a matching authorization file to a privileged user account on the other
system:

```
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod go-rwx ~/.ssh/*
cd ~/.ssh
scp id_rsa.pub id_rsa authorized_keys user@node2:
chown postgres.postgres authorized_keys id_rsa.pub id_rsa
mkdir -p ~postgres/.ssh
chown postgres.postgres ~postgres/.ssh
mv authorized_keys id_rsa.pub id_rsa ~postgres/.ssh
chmod -R go-rwx ~postgres/.ssh
```

Now test that ssh in both directions works.  You may have to accept some new
known hosts in the process.
