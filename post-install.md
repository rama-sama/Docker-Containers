# InfluxDB
```
cd /opt/homeassistant/config/
sudo vim configuration.yaml
```

Add the following
```
recorder:
  db_url: !secret mariadb
  purge_keep_days: 10   # default

history:

influxdb:
  api_version: 2
  ssl: false
  host: !secret influxdb_host
  port: 8086
  token: !secret influxdb_token
  organization: !secret influx_org
  bucket: homeassistant
  tags:
    source: HomeAssistant
  tags_attributes:
    - friendly_name
  default_measurement: units
  ignore_attributes:
    - icon
  exclude:    # Customise to fit your needs
    entities:
      - zone.home
    domains:
      - persistent_notification
      - person
```

Set the following envirionment variables via `.env` file
```
INFLUXDB_HOST="<host>"
INFLUXDB_TOKEN="<influxdbtoken>"
INFLUXDB_ORG="<influxorg>"
```

Add the following to secrets.yaml
```
mariadb: "mysql://homeassistant:mariadbhapassword@mariadbhost:3306/ha_db?charset=utf8mb4"

influxdb_host: "influxdbhost"
influxdb_token: "influxdbtoken"
influx_org: "influxorg"
```

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

Mosquitto log should show container restarted
```
sudo cat /opt/mosquitto/log/mosquitto.log
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
