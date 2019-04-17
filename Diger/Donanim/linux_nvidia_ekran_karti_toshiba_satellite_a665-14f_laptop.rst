..code:: sh

        lspci |grep VGA

01:00.0 VGA compatible controller: NVIDIA Corporation GT215M [GeForce GTS 350M]
(rev a2)


..code:: sh

        sudo ubuntu-drivers devices

        == /sys/devices/pci0000:00/0000:00:1c.1/0000:07:00.0 ==
        model    : BCM4313 802.11b/g/n Wireless LAN Controller
        vendor   : Broadcom Corporation
        modalias : pci:v000014E4d00004727sv0000144Fsd00007175bc02sc80i00
        driver   : bcmwl-kernel-source - distro non-free
        
        == /sys/devices/pci0000:00/0000:00:03.0/0000:01:00.0 ==
        model    : GT215M [GeForce GTS 350M]
        vendor   : NVIDIA Corporation
        modalias : pci:v000010DEd00000CB0sv00001179sd0000FD30bc03sc00i00
        driver   : nvidia-304-updates - distro non-free
        driver   : xserver-xorg-video-nouveau - distro free builtin
        driver   : nvidia-331-updates - distro non-free
        driver   : nvidia-304 - distro non-free
        driver   : nvidia-331 - third-party non-free recommended
