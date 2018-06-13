# prometheus-influx-raspberrypi



## Create influxdb docker container and create a database 

### Create influx container

```
docker run -p 8086:8086 -v /var/lib/influxdb:/var/lib/influxdb influxdb --name <INFLUX-CONTAINER-NAME>
```

### Create a database

```
docker exec -it <influx_container-id> influx
```

```
CREATE DATABASE <PROMETHEUS_DB_NAME>
```


## Create the prometheus armv7 docker image -Tested on RPi 3b+

### Create a prometheus arm docker image using Dockerfile from this repo

```
docker build -t  <docker-hub-name>/prometheus-arm64:v2.2.1 .
```

### Create a prometheus configuration file with REMOTE_WRITE details pointing to above influx container

```
vi prometheus.yaml
```
```
global:
 scrape_interval: 1s
 scrape_timeout: 1s
scrape_configs:
 - job_name: prometheus
   static_configs:
    - targets: ['localhost:9090']
remote_write:
 - url: http://<INFLUX-CONTAINER-NAME>:8086/api/v1/prom/write?db=prometheus
```

### Create prometheus container using the above image and bind it to influx container

```
docker run -d -p 9090:9090 -v <COMPLETE_PATH_TO_CONFIG_FILE>/prometheus.yml:/etc/prometheus/prometheus.yml --name=prometheus --link <INFLUX-CONTAINER-NAME>:influxdb <docker-hub-name>/prometheus-arm64:v2.2.1 --config.file=/etc/prometheus/prometheus.yml
```
