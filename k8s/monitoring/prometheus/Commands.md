* Prometheus offers hot reloads. So if you need to update the config or rules
file, just update the config map and make a HTTP POST request to the
prometheus endpoint. Eg:
```
curl -XPOST http://<Prometheus-Svc-Ext-Ip>:8080>/-/reload 
```
logs: Completed loading of configuration file filename=/etc/prometheus/prometheus.yml

* Alertmanager config can be reloaded by a similar api call.
```
curl -XPOST http://<Alertmanager-Svc-Ext-Ip>:9093>/-/reload
```
