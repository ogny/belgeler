### Crm Resource'lari ile cluster yapılandırması


* Gecmis konfigurasyonu degistiremedim, silerek yeniden olusturdum.
`crm configure erase`

#### Karisik notlar
* crm help Overview
* crm help topics

  primitive apache apache \
    params configfile=/disk0/etc/apache2/site0.conf

crm ls
cibstatus        help             site             
cd               c            test_user="test_user" test_passwd="2JcXCxKF" \7

* Gruba yeni resource ekleme
```
crm configure show Grp1 | sed 's/$/ newrsc/' | crm configure load update -
```

* Grupta duzenleme
```
crm configure property maintenance-mode=true
crm resource stop R1 # it won't stop as it's in maintenance-mode
crm configure delete R1
crm configure show # very that all references to R1 are gone
crm resource reprobe # the cluster double check the status of declared 
sources and sees that everything is fine and R1 doesn't exists anymore
crm_mon -Arf1 # double check that everything is "started (unmanaged)" d R1 is gone
crm_simulate -S -L -VVV # optional, to check what would happen when  aving maintenance-mode
crm configure property maintenance-mode=false
```
