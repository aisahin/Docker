# Docker

## InfluxDB and Grafana
```sh
docker volume create influxdb_data
docker volume create grafana_data
docker network create monitor_network
docker-compose up -d
```