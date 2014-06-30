* Yeni acilmis repo'ya ilk push'u yapmadan once asagidaki komut verilir;  
git push origin master  

* git'te her zaman calisan surum tutulur, gelistirme branch'inda da olsan  
* calismayan surumu push etmezsin.  

* bare repo'da ilk commit'te duzeltme  
git push --set-upstream origin master  

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
```
### Rebase

* commit log'larinin anlasilabilir olmasi icin rebase kullan.
```
git pull --rebase  
git push  
conflict olusursa;  
git rebase --continue  
```
#### Uzak branch'lar
* yerelde olusturdugumuz serverconfig branch'ini uzak repoya yuklemek icin;
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

#### Cesitli hatalar;
`fatal: The current branch master has multiple upstream branches, refusing to
push.`  
```
git config remote.origin.push HEAD  

```

