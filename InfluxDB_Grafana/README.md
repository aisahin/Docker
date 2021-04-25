# InfluxDB and Grafana
**Docker Host:** 
- Debian GNU/Linux 10 (buster)
**Images:** 
- influxdb:latest (2.0.4, Debian GNU/Linux 10 (buster))
- grafana/grafana:latest (7.5.4, Alpine Linux)

> **Note:** You should change resource allocation and network specification based on your needs for containers.

## Before deployment
1. Pull related docker images.
```sh
sudo docker pull influxdb
sudo docker pull grafana/grafana
```
2. Create volumes.
```sh
sudo docker volume create influxdb_data
sudo docker volume create influxdb_config
sudo docker volume create grafana_data
```
3. Create network.
```sh
sudo docker network create monitoring --driver=bridge --subnet=172.18.1.0/24 --ip-range=172.18.1.0/24 --gateway=172.18.1.1
```

## Deployment
1. Bring the images up. 
```sh
sudo docker-compose --env-file ./config/.env.dev up -d
```

## Post deployment 
Influx initial setup.

1. Connect your influxdb container. 
```sh
sudo docker exec -it $(sudo docker container ls | grep influxdb | awk '{print $1}') /bin/bash
```
2. Create your organization, bucket and user with password.
```sh
influx setup --org example-org  --bucket example-bucket --username admin  --password changeme --force
```
3. First get your bucket id and create authentication for user to bucket.
```sh
influx bucket list | grep example-bucket | awk '{print$1}'
```
```sh
influx v1 auth create --username telegraf --password changeme  --org example-org  --read-bucket {BUCKET_ID} --write-bucket {BUCKET_ID} -d "Telegraf token"
```
4. Create database.
```sh
influx v1 dbrp create --db example-db --rp example-rp --bucket-id {BUCKET_ID}
```

## References
- [Influxdb Docker image](https://hub.docker.com/_/influxdb)
- [Grafana Docker image](https://hub.docker.com/r/grafana/grafana)
- [Running InfluxDB 2.0 and Telegraf Using Docker](https://www.influxdata.com/blog/running-influxdb-2-0-and-telegraf-using-docker/)
- [Use Grafana with InfluxDB OSS](https://docs.influxdata.com/influxdb/v2.0/tools/grafana/?t=InfluxQL#view-and-create-influxdb-dbrp-mappings)