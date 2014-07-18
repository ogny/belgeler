### Platform
`debian wheezy amd64 3.2.0-4`
### Kurulum
* debian/jessie ve ubuntu/utopia'da default olarak geleceginden kurulum
belirtilmeyecek

### Tanimlar
* PID 1'i alir ve ilk boot process'i olarak calisir, init system'inin yerini alir. 

CONCEPTS
    systemd provides a dependency system between various entities called "units".
five generalized unit states
1. Service units, which control daemons and the processes they consist of 
2. Socket units, which encapsulate local IPC or network sockets in the system
3. Target units are useful to group units
4. Device units expose kernel devices in systemd 
5. Mount units control mount points in the file system

Application programs and units (via dependencies) may request state changes of
units. In systemd, these requests are encapsulated as 'jobs' and maintained in
a job queue. Jobs may succeed or can fail, their execution is ordered based on
the ordering dependencies of the units they have been scheduled for.

systemd has a minimal transaction system: if a unit is requested to start up or
shut down it will add it and all its dependencies to a temporary transaction.

Systemd contains native implementations of various tasks that need to be
executed as part of the boot process. For example, it sets the host name or configures the loopback network device. It also sets up and mounts various API file systems, such as /sys or /proc

DIRECTORIES
     The systemd system manager reads unit configuration from various
directories. Packages that want to install unit files shall place them
    System unit directories `/usr/lib/systemd`
        

ENVIRONMENT
    $SYSTEMD_UNIT_PATH: Controls where systemd looks for unit files.
    $LISTEN_PID, $LISTEN_FDS: Set by systemd for supervised processes during socket-based activation. See sd_listen_fds(3) for more information.
    $NOTIFY_SOCKET: Set by systemd for supervised processes for status and start-up completion notification.

SOCKETS AND FIFOS
    /run/systemd/shutdownd: Used internally by the shutdown(8) tool to implement delayed shutdowns

### Moduller
#### systemd.automount: unit configuration file whose name ends in .automount encodes information about a file system automount point controlled and supervised by systemd.
Automount units must be named after the automount directories they control.
Example: the automount point /home/lennart must be configured in a unit file home-lennart.automount.

#### systemd-cat 
Connect a pipeline or program's output with the journal 

systemd-analyze
systemd-analyze blame
systemd-analyze plot > plot.svg
* [ ] screen ve tmux X'i restart ettigimde oluyordu:
sudo echo "session required pam_systemd.so" >>  /etc/pam.d/slim
* [ ] umask 0000 yaptigini yaz

systemd.socket
Socket units may be used to implement on-demand starting of services, as  well
as parallelized starting of services
Socket files must include a [Socket] section

### Systemctl
* reload
 Asks all units listed on the command line to reload their configuration. Note that this will reload the service-specific configuration, not the unit configuration file of systemd. If you want systemd to reload the configuration file of a unit use the daemon-reload command. In other words: for the example case of Apache, this will reload Apache's httpd.conf in the web server, not the apache.service systemd unit file.
 This command should not be confused with the daemon-reload or load commands.
* status
tum unit'lerin durumu gorulebilir.
* show
unit'ler hakkinda detayli bilgi verir.
* reset-failed
Reset the 'failed' state of the specified units, or if no unit name is passed
of all units. When a unit fails in some way (i.e. process exiting with non-zero
error code, terminating abnormally or timing out) it will automatically enter the 'failed' state and its exit code and status is recorded for introspection by the
administrator until the service is restarted or reset with this command.
* list-unit-files
* mask
Mask one or more unit files, as specified on the command line. This will link these units to /dev/null, making it impossible to start them. This is a stronger version of disable, since it prohibits all kinds of activation of the unit, including manual activation. Use this option with care.
* unmask [NAME...]
Unmask one or more unit files, as specified on the command line. This will undo the effect of mask.

* daemon-reload
Reload systemd manager configuration. This will reload all unit files and
recreate the entire dependency tree. While the daemon is reloaded, all sockets
systemd listens on on behalf of user configuration will stay accessible.

### Notlar
Opensuse sunucularda (3.1.10-1.29)
systemd-cgtop komutu yok








