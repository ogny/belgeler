dmesg ciktisinda su hatayi goruyorum;

firmware: requesting ql2500_fw.bin
Failed to load firmware image (ql2500_fw.bin).
Fimware image unavailable.
Firmware images can be retrieved from:
http://ldriver.qlogic.com/firmware/.

Olmasi gereken bu;
firmware: requesting ql2500_fw.bin 
FW: Loading via request-firmware... 
Allocated (64 KB) for FCE... 
Allocated (64 KB) for EFT...
Allocated (1350 KB) for firmware dump... scsi2 : qla2xxx


Kendim elle ql2500_fw.bin 'i sunucuya yukleyip gosterdigimde;

# /opt/QLogic_Corporation/QConvergeConsoleCLI/qaucli -fc -b all /lib/firmware/ql2500_fw.bin'

IBM VirtualDisk (scsi)
IBM-ESXS Model: ST9146852SS

multipathd ayarlari
dmsetup ls --target=multipath
cat /proc/scsi/scsi
dmesg | grep -i "attached"
multipath -l
cat /proc/partitions
fdisk -l
ll /sys/block
multipath -l
df -h
