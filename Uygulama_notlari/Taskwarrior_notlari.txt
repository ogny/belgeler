## Taskd server ile taskwarrior cli ve Android App.'i sync kurulumu
### Client Platform  
`Ubuntu 14.04 trusty 3.13.0-29-generic x86_64`  
### Server  Platform  
`Debian Jessie/sid 3.2.0-4-amd64`  
#### Her iki sistem icinde kurulacak paketler   
```  
sudo apt-get install cmake uuid-dev libgnutls-dev build-essential -y  
```  
#### Sunucuya taskd Kurulumu  
* [Mirakel'in sayfasindaki](http://mirakel.azapps.de/taskwarrior.html) adimlar  
uygulanir.  
* <taskd_kurulu_dizin>/demo/server/ altinda olusan config dosyasi  Mirakel  
Android app'a ve taskwarrior kurulacak client'a kopyalanir.  
  
#### Client'a task Kurulumu  
* task uygulamasinin debian-based OS'lardaki paketlenmis surumunde sync  
ozelligi eklenmemis, bu sebepten yazilimci firmanin dagittigi surumu  
kuracagiz;  
```  
cd /tmp  
wget http://taskwarrior.org/download/task-2.3.0.tar.gz  
tar xzf task-2.3.0.tar.gz  
cd task-2.3.0  
cmake .  
make && sudo make install  
```  
* [Taskwarrior'un sayfasindaki](taskwarrior.wordpress.com/tag/2-3-0/) Client  
Setup adimlari uygulanir. (kullanici key'leri Mirakel Android App icin daha  
once olusturuldugundan olusturma (generate) adimlari atlanir,   
#### VIT curses arayuzu kurulumu  
```  
git clone https://git.tasktools.org/scm/ex/vit.git  
./configure  
make && sudo make install  
```  

#### zsh completion'u icin;
[zshrc'ye
eklenecek](https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/plugins/taskwarrior/taskwarrior.plugin.zsh)

## Taskwarrior Kullanim
### Projects
* Note that a task may only belong to one project.
* Belli bir projede calismak icin;
`task pro:<proje_adi> li`

### Tags
* A task may have any number of tags, which are just single words associated with the task
* The long report shows tags
`$ task long`
