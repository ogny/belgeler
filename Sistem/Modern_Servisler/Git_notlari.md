* Yeni acilmis repo'ya ilk push'u yapmadan once asagidaki komut verilir;  
```
git push origin master
```

* git'te her zaman calisan surum tutulur, gelistirme dalinda da olsan   calismayan surumu push etmezsin.  
* bare repo'da ilk commit'te duzeltme  
```
git push --set-upstream origin master  
```

### dallarla calisma;
* bulundugun dali gorme;
```
git branch -v
```
* Yeni bir dal acip ona gecmenin kisayolu
```
git checkout -b <dal_adi>
```
* dali silme; (eger silmeye izin vermiyorsa -D ile cagir)
```
git branch -d <dal_adi>
```

* yerel ve uzak dallari gorme;
```
git branch -a
```

* yeni uzak dal acma

### rebase kullanimi
* iki dalin var fix_dali ve master_dali. fix_dalinda guncelleme yaptin,
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

#### Uzak dallar
* yerelde olusturdugumuz dali uzak repoda ayni dal olarak yuklemek icin;
```
git push origin <dal_adi>:<dal_adi>
```
* Uzak repo'daki dali silmek;
```
git push origin :<dal_adi>
```

* Tracking
```
git branch --set-upstream-to=origin/<uzak_dal> <lokal_dal>
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

stash ile degisiklikleri zulaliyosun.
stash apply ile restore ediyosun.

* coklu repo guncelleme

for REPO in `ls`; do (cd "$REPO"; git pull); done;

### Github
* yeni eklenen bir repo'nun her commit'te parola sormamasi icin
```
git remote set-url origin git@github.com:
```
* yeni acilan repo'nun yerel makinalara eklenmesi
create a new repository on the command line  
```
touch README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/ogny/anlatabildiklerim.git
git push -u origin master
```
or push an existing repository from the command line  
```
git remote add origin https://github.com/ogny/anlatabildiklerim.git
git push -u origin master
```

* son commit'le onceki arasindaki farklari gorme;
```
git diff --color HEAD^ HEAD
git log -p <dosya_adi>
```
```
git log --color -p
```

* repo dizininde remote'a gonderilmesini istemedigimiz degisiklikleri geri
  alma;
```
git clean -df
```

* pretty log format
```
git log --pretty=format:user:%aN%n%ct --reverse \ 
--raw --encoding=UTF-8 --no-renames --no-merge
```

* tum degistirilmis dosya/dizinleri ekleyemediginde, sorun yeni eklenen bir
  dizindeki .git dizininden kaynaklanabilir.
```
rm -rf subfolder/.git
git add -A
```


