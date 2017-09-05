* parolasiz kullanici login'de olusan hata; `windows7 logon failure user 
  account
restriction possible reason are blank passwords not allowed`
cozumu: 
  - Click the Start button and type gpedit.msc in the Search programs and files 
    bar and hit enter.
  - At the left pane, go to Local Computer Policy > Computer Configuration > 
    Windows Settings > Security Settings > Local Policies > Security Options
  - Look for “Accounts: Limit local account use of blank passwords to console 
    logon only” and double click on it.
  - By default the Enable option is selected and all you need to do is select 
    “Disable” and click OK.

[kaynak](https://sqlcurve.wordpress.com/2013/05/21/user-account-restriction-possible-reasons-are-blank-passwords-not-allowed-logon-hour-restrictions-or-policy-restriction-has-been-enforced/)

* rdesktop ile erisim'de hata;
```
windows rdp ERROR: CredSSP: Initialize failed, do you have correct kerberos tgt 
initialized ? Failed to connect, CredSSP required by server.
cozumu: Network Level Access 'i disable et.

* acilista baslamasini istedigin script'i koyacagin lokasyon;
```
run > shell:startup
```

* sunucunun uptime'i;
```
run > cmd > net stats srv
```

* Windows10'da uyku moduna gecince network'ten erisim kesiliyordu. Cozum;
```
Control Panel > Hardware and Sound > Power Options > Change > Put the computer 
to sleep > Newver
```
* windows10 ethernet unidentified network "the default gateway is not available"
WMware'de ethernet type'ini e1000 seciyoruz, default ethernet driver'ini "intel
pro 1000 mt" atiyor. kesinti yasanmiyor.
