* calistirma (root veya kullaniciya gecis yap)

```
java -cp /usr/lib/riemann/riemann.jar:/usr/lib/riemann/riemann-extra-0.2.4.jar \
riemann.bin start /etc/riemann/riemann.config > /dev/null 2>&1 &

nohup java -cp lib/riemann.jar:lib/riemann-extra-0.2.4.jar \
riemann.bin start etc/riemann.config > /dev/null 2>&1 &
```

* Metric'ler akarken hangisinin yakalanacaginin tanimi (tagging)
temp_tag_with_threshold_check.config

* thresholds.config'te tag'lananlarin filtrelenecegi araliklari veriyoruz. (trigger)
thresholds.config

* tag'lanan event'lerin uygun filtrelerle yakalandiginda ne yapilacak (action)
threshold_check.config



