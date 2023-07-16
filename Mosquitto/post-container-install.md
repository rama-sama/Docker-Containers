## Mosquitto.conf
```
cd /opt/mosquitto/config/
sudo wget https://raw.githubusercontent.com/eclipse/mosquitto/master/mosquitto.conf
```

Add the following lines to the end of mosquitto.conf
```
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

## Mosquitto users
```
docker exec -it mosquitto mosquitto_passwd -c /mosquitto/config/mqttuser homeassistant
```

```
cat /opt/mosquitto/config/mqttuser
```

```
docker compose restart mosquitto
```
