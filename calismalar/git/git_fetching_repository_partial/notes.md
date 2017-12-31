Sonunda git grubunda bu konuda bugun bir paylasim yayinlandi;
https://groups.google.com/forum/#!topic/git-users/sBX1-a8I5v0
Buna gore;

```bash
git  clone --no-checkout --depth 1
https://github.com/emrahcom/www_emrah_com.git
cd ​www_emrah_com
git config core.sparseCheckout true
echo "public_html/notlar/*" >> .git/info/sparse-checkout
git checkout 
```
