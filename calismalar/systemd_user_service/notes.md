#### Case
tmux'u Systemd ile user icin daemon olusturdugumda acilista baslamiyor
case'in dogrulanmasi icin evde makina restart edilip kontrol edilecek.

#### Workarounds
* type:oneshot
Configures the process start-up type for this service unit. One of simple,
forking, oneshot, dbus, notify or idle.
Oneri: tmux_new adiyla yeni bir tane duzenledim, onu enable edip test et.

* ExecStart'taki -s %H parametresi sorun cikartiyor olabilir mi?
Oneri: baska hangi parametreler olabilir, elle mi versek?

buradaki ayarlar kontrol edilmeli:
[Why doesn't my systemd user unit start at boot?](https://unix.stackexchange.com/questions/251211/why-doesnt-my-systemd-user-unit-start-at-boot)
