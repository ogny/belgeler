================
Df Kullanimi
================

- Takili harddiskteki bos yerleri ve kullanimda olan alanlarin oranini gösterir.

.. code:: sh

        df -h


Ornek komut ciktisi;

.. code:: sh

    Filesystem     1K-blocks     Used Available Use% Mounted on
    /dev/sda2       40322424  9372776  28901344  25% /
    udev               10240        0     10240   0% /dev
    tmpfs            1183020     9260   1173760   1% /run
    tmpfs            2957548    40728   2916820   2% /dev/shm
    tmpfs               5120        4      5116   1% /run/lock
    tmpfs            2957548        0   2957548   0% /sys/fs/cgroup
    /dev/sda6      261550244 25713200 222551004  11% /home
    tmpfs             591512        4    591508   1% /run/user/1000



* An inode is what the Linux file system uses to identify each file. When a file
  system is created (using the mkfs command), the file system is created with a
  fixed number of inodes. If all these inodes become used, a file system cannot
  store any more files even though there may be free disk space
