#### Yapilacaklar

* merge backed or reverted (bunu daha once kullanmamistim, test edecegim)

* birden cok dosyada degisiklik yaparsan, `git add dosya1 && git commit -m
"1.dosya" seklinde duzgun log tutarak git'e gonder

* log'lari izleme
```
git log         \        
    --oneline   \
    --graph     \
    --all       \ 
    --decorate  
```


* repo dizininde remote'a gonderilmesini istemedigimiz degisiklikleri geri alma;(reset ve clean ile)
```
git clean -df
```

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
* iki dalin var fix_dali ve master_dali. fix_dalinda guncelleme yaptin, test ettin, artik master_dalina uygulayabilirsin. 
```
git checkout master_dali
git rebase fix_dali
```
* uzak repo'da ayni dalda baskalariyla calisiyorsan senden onceki rebase'leri cek;
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
`fatal: The current branch master has multiple upstream branches, refusing to push.`  
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
git diff --color Head~1..
git diff --color HEAD^ HEAD
git log -p <dosya_adi>
```
```
git log --color -p
```


* pretty log format
```
git log --pretty=format:user:%aN%n%ct --reverse \ 
--raw --encoding=UTF-8 --no-renames --no-merge
```

* tum degistirilmis dosya/dizinleri ekleyemediginde, sorun yeni eklenen bir dizindeki .git dizininden kaynaklanabilir.
```
rm -rf subfolder/.git
git add -A
```
[kaynak:StackOverFlow](http://stackoverflow.com/questions/7726131/git-add-a-is-not-adding-all-modified-files-in-directories)

* temizlenen repolarla çalışırken öncesinde `git clean -fd` çalıştırın pyc
  dosyalarından dolayı silinmemiş gibi görünüyor

* Getting the difference between two repositories
The scenario: We have a repo_a and repo_b. The latter was created as a copy of
repo_a. There have been parallel development in both the repositories
afterwards. Is there a way we can list the differences of the current versions
of these two repositories? solution: In repo_a:
```
git remote add -f b path/to/repo_b.git
git remote update
git diff master remotes/b/master
git remote rm b
```
### Genel Kullanım

* refs/heads tanımla.
* hook nedir, nerede kullanılıyor? 
* Pull Request'in kullanım amacını tanımla.

#### Geri alım senaryoları
* yerel dal'ta üzerinde çalışılan, fakat uzak dal'a eklenmemiş dosyaların
* uzak dal'a eklenmiş dosyaların durumu:
* uzak dal'a commit'lenmiş dosyaların durumu:

#### Git Geliştirmeleriyle çalışma 

#### Tag'larla çalışma
```
git tag
git checkout tags/0.9.15
```

##### dal'lerle çalışma 


* hangi dal'te olduğunu kontrol et: `git branch`
* yerel dal'ına geç: `git checkout <yerel_dal>`
* çalışmanı yerel dal'ına gönder: 
```
git add . 
git commit -m "<açıklama>"
```
* çalışmanı uzak dal'ına gönder: `git push origin <uzak_dal>`
* birleştireceğin dal'a geç, değişimleri al:
```
git checkout <asıl_dal>
git fetch --all && git pull origin <asıl_dal>
```

* uzak dal'ını asıl dal'la birleştir:
```
git merge --no-ff <uzak_dal>
git push origin  <asıl_dal>
```

###### Merge/Rebase
* Dosyada yaptığınız değişikliği geri alıp pull alınca auto-merge yapıyor. Bunda bir problem yok. Adımlar:
geri alma: `git checkout .` 
pull alma, uyari: `Auto-merging <dosyanin_yolu>`

##### Flow örnek çalışma eklenecek.

```
$ git flow init -d                                           
Branch name for production releases: [master] 
Branch name for "next release" development: [develop] 
Feature branches? [feature/] 
Release branches? [release/] 
Hotfix branches? [hotfix/] 
Support branches? [support/] 
Version tag prefix? [] 
```

* [git flow cheatsheet](http://danielkummer.github.io/git-flow-cheatsheet/)
* [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
* [Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow/)

#### Global kullanılabilecek convention'lar

##### gitconfig Örnekleri 
##### gitalias  Örnekleri 
##### gitignore Örnekleri

#### Log 
```
git log --oneline --graph -4 --decorate 
git log --no-merges -p --function-context --pretty=format:user:%aN%n%ct \
--reverse --encoding=UTF-8 --no-renames
```

#### Reset
```
git reset --soft HEAD~n veya git reset --soft commit_id
```

Commit kayıtlarında "n" (1,2,3...) kere geri gider ve değiştirilen tüm
dosyaları index alanına geri koyar. O andan onceki ve sonraki tüm commit
kayıtlarındaki değişiklikler olduğu gibi kalır. Örnek: eğer 3. committen 1.
commite geri dönülüp, tekrar commit işlemi gerçekleştirilirse, commit 2 ve 3
kayıtlarından kaldırılır çünkü onlar artık commit 1'in içine geçmiş olurlar.

#### Kullanışlı araçlar
* cherry-pick
* rebase

…or create a new repository on the command line
echo "# acik_notlar" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:haticey/acik_notlar.git
git push -u origin master
…or push an existing repository from the command line
git remote add origin git@github.com:haticey/acik_notlar.git
git push -u origin master

#### Kaynaklar
* [Ali Özgür](https://aliozgur.gitbooks.io/git101/content)
* [Seçkin Alan](http://sckn.org/git_notlari.txt)
* [Emrah Eryılmaz](https://raw.githubusercontent.com/emrahcom/www_emrah_com/master/public_html/notlar/git_notlari.txt)
* [Muhammet Macit](https://sekomy.atlassian.net/wiki/pages/viewpage.action?spaceKey=DEV&title=Editing+Template+Repo)
* [inanzzz](http://www.inanzzz.com/index.php/yazi/9j27/en-yaygin-sekilde-kullanilan-git-komutlari)
* [Git Görsellestirme](http://marklodato.github.io/visual-git-guide/index-en.html)
