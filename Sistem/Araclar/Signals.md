* trap signals
```
kill -0 $(pidof beam.smp)
```
    - Sinyali gonderdigin bir daemon'sa -HUP ile reload edebilirsin:
```
killall -HUP bluetoothd
```

* Process kill signals. 

    - Sinyali gonderdigin bir daemon degilse -HUP process'i oldurur


