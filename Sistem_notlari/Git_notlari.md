* Yeni acilmis repo'ya ilk push'u yapmadan once asagidaki komut verilir;  
```
git push origin master
```

* git'te her zaman calisan surum tutulur, gelistirme branch'inda da olsan   calismayan surumu push etmezsin.  
* bare repo'da ilk commit'te duzeltme  
```
git push --set-upstream origin master  
```

### Branch'larla calisma;
* bulundugun branch'i gorme;
```
git branch -v
```
* Yeni bir branch acip ona gecmenin kisayolu
```
git checkout -b <branch_adi>
```
* branch'i silme;
```
git branch -d <branch_adi>

* yerel ve uzak branch'lari gorme;
```
git branch -a
```

### rebase kullanimi
* iki branch'in var fix_dali ve master_dali. fix_dalinda guncelleme yaptin,
* test ettin, artik master_dalina uygulayabilirsin. 
```
git checkout master_dali
git rebase fix_dali
```
* uzak repo'da ayni dalda baskalariyla calisiyorsan senden onceki rebase'leri
cek;
```
git pull --rebase  
git push origin master 
```
* Problem oldugu durumda
* problemi diff'le gorup dosyayi editleyip cozelim.
```
git rebase --continue  
```

#### Uzak branch'lar
* yerelde olusturdugumuz dali uzak repoda ayni dal olarak yuklemek icin;
```
git push origin <branch_adi>:<branch_adi>
```
* Uzak repo'daki branch'i silmek;
```
git push origin :<branch_adi>
```

* Tracking
```
git branch --set-upstream-to=origin/<uzak_branch> <lokal_branch>
```

### git-flow kullanimi
* Kurulum ve kullanim icin [nvie'nin
sayfasindan](https://github.com/nvie/gitflow) notlar cikartilacak
* Not: git-flow en onemli konular arasinda, flow cikartilacak

#### Cesitli konular;
`fatal: The current branch master has multiple upstream branches, refusing to
push.`  
```
git config remote.origin.push HEAD  
```
* refs/heads/master 'daki gecisleri gormek icin;
`git reflog`

*  git-blame - Show what revision and author last modified each line of a  file
#### stash (kelime anlami, zulalamak_
    Calling git stash without any arguments is equivalent to git stash save
    The latest stash you created is stored in refs/stash


* coklu repo guncelleme

for REPO in `ls`; do (cd "$REPO"; git pull); done;

### Github
* yeni eklenen bir repo'nun her commit'te parola sormamasi icin
```
git remote set-url origin git@github.com:
```
