==============
Ranger Calisma
==============

* kisayollar icin 1?

Kisayollar
----------

* Toplu silme;
  space ile silinecek dizin/dosyalari secip :delete ile sil

yon:
G ve gg
<c-u> move up=0.5    pages=True
<c-d> move down=0.5  pages=True 

<enter> move right=1  
/ console search
f console find 
q quit
r console open_with 
s console shell 
V toggle_visual_mode 
dc get_cumulative_size
zh toggle_option show_hidden

H history_go -1                                                       
L history_go 1

# yeniden adlandirirken basa ve sona ekleme
A eval fm.open_console('rename ' + fm.env.cf.basename)  
I eval fm.open_console('rename ' + fm.env.cf.basename, position=7)

# cut mode
da cut mode=add
dr cut mode=remove
dd cut
uy uncut
ud uncut

# paste mode
po paste overwrite=True
pp paste

# paste mode
ya copy mode=add
yr copy mode=remove
yy copy


