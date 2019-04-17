```
mount: unknown filesystem type 'LVM2_member'
```

parted ile LVM label'i tanimlamak icin 


lvmdiskscan



[LVM Administration with CLI Commands](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Logical_Volume_Manager_Administration/LVM_CLI.html)

* Disk silme;
```
e2fsck -f /dev/mapper/jua-root
resize2fs /dev/mapper/jua-root 100G
lvreduce /dev/mapper/jua-root -L 110G
resize2fs /dev/mapper/jua-root
pvmove /dev/mapper/wd (I think)
vgreduce jua /dev/mapper/wd
```
