		


	Debian wheezy'de intel ses karti-alsa surucusuyle ilgili sorun olursa:
		sudo lspci |grep -i audio
		> NM10/ICH7 Family High Definition Audio Controller (rev 02)
		sudo vim /etc/modprobe.d/alsa-base.conf
		# To fix this I add to the config file:
		options snd-hda-intel model=auto
		sudo alsa force-reload 
	Kaynak: http://ubuntuforums.org/showthread.php?t=1860789	
