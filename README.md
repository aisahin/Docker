# Docker

## InfluxDB and Grafana
```sh
sudo docker volume create influxdb_data
sudo docker volume create influxdb_config
sudo docker volume create grafana_data
sudo docker network create monitoring --driver=bridge --subnet=172.18.1.0/24 --ip-range=172.18.1.0/24 --gateway=172.18.1.1
sudo docker-compose --env-file ./config/.env.dev up -d
```