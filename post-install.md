```
sudo chown -R 1000:1000 /opt/nodered/data
```

Add the following to /opt/homeassistant/configuration.yaml
```
panel_iframe:
  nodered:
    title: Node-RED
    icon: mdi:lan
    url: http://192.168.10.106:1880/
    require_admin: true
```
