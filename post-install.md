# Mosquitto
```
cd /opt/mosquitto/config/
sudo wget https://raw.githubusercontent.com/eclipse/mosquitto/master/mosquitto.conf
```

```
# add the following lines at the end

# Listen on port 1883 on all IPv4 interfaces
listener 1883
socket_domain ipv4
# save the in-memory database to disk
persistence true
persistence_location /mosquitto/data/
# Log to stderr and logfile
log_dest stderr
log_dest file /mosquitto/log/mosquitto.log
# Require authentication
allow_anonymous false
password_file /mosquitto/config/mqttuser
```

Mosquitto users
```
docker exec -it mosquitto mosquitto_passwd -c /mosquitto/config/mqttuser homeassistant
```

Check Mosquitto users
```
cat /opt/mosquitto/config/mqttuser
homeassistant:$7$101$q7VtJJ/E*******7$1I******************************************b/G**************************************A==
```

Restart Mosqutto
```
docker compose restart mosquitto
```

# Node-RED
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
