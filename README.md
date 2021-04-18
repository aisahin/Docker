# Docker

## InfluxDB and Grafana
```sh
docker volume create influxdb_data
```

```sh
docker volume create grafana_data
```

```sh
docker network create monitor_network
```

```sh
docker-compose up -d
```