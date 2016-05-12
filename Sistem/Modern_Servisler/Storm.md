* Storm Nimbus
```
./storm/bin/storm nimbus 2>&1 > storm.out&
```

* Storm Supervisor
```
./storm/bin/storm supervisor 2>&1 > storm.out&
```

* Storm ui
```
./storm/bin/storm ui 2>&1 > ui.out &
```

* Storm
```
./storm/bin/storm jar <paket.jar> -Dsnmp.properties=file://$HOME/snmp.properties <com.<paketadi>.SnmpTopology> snmp-201605091750
```
