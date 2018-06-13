# prometheus-influx-raspberrypi

## Create influxdb docker container and create a database 

### Create influx container

```
docker run -p 8086:8086 -v /var/lib/influxdb:/var/lib/influxdb influxdb --name <INFLUX-NAME>
```

### Create a database

```
docker exec -it <influx_container-id> influx
```

```
CREATE DATABASE <PROMETHEUS_DB_NAME>
```

## Create the prometheus armv7 docker image -Tested on RPi 3b+

### Create a prometheus arm docker image

```

```
