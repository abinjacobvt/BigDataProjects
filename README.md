# Air Quality Monitoring and Pollution Forecasting
Big Data & Distributed Processing Project

---

## Project Overview

This project implements a distributed Big Data pipeline to monitor and analyze Air Quality Index (AQI) data for major French cities: Paris, Lyon, and Marseille.

The system demonstrates an end-to-end Kafka → Spark → HDFS → InfluxDB → Grafana architecture deployed on a distributed Hadoop/Spark cluster. AQI data is ingested from an external REST API, processed using Spark Structured Streaming and batch jobs, stored in HDFS and InfluxDB, and visualized using Grafana dashboards.

The project showcases core Big Data concepts including distributed ingestion, streaming processing, scalable storage, fault tolerance, and time-series visualization.

---

## System Architecture

AQI REST API  
→ Python Producer  
→ Kafka Topic (airquality.raw)  
→ Spark Structured Streaming  
→ HDFS (Raw AQI Data)  
→ Spark Batch Processing  
→ InfluxDB (Daily AQI KPIs)  
→ Grafana Dashboard

---

## Technologies Used

- Apache Kafka – Distributed data ingestion  
- Apache Spark (Structured Streaming & Batch) – Data processing  
- Apache Hadoop HDFS – Distributed storage  
- InfluxDB – Time-series database  
- Grafana – Data visualization dashboard  
- Python – Producer and Spark jobs  
- Linux Distributed Cluster – Master / Worker setup  

---

## Cluster Setup

| Component | Node |
|--------|------|
| Zookeeper | Master |
| Kafka Broker | Master |
| Spark Driver | Master |
| Spark Executors | Worker |
| HDFS NameNode | Master |
| HDFS DataNode | Worker |
| YARN ResourceManager | Master |
| InfluxDB | Master |
| Grafana | Master |

---

## Data Source

- Provider: World Air Quality Index (WAQI)
- API Endpoint:  
  https://api.waqi.info/feed/{city}/?token=YOUR_TOKEN
- Cities: Paris, Lyon, Marseille
- Metrics: AQI, PM2.5, PM10, NO₂, O₃, CO, SO₂
- Data Frequency: Daily execution

---

## Repository Structure

BigDataProjects/
├── producer/
│   └── aqi_producer.py
├── spark/
│   ├── kafka_to_hdfs.py
│   └── daily_kpi_to_influx.py
├── .gitignore
└── README.md

---

## Pipeline Execution

### Start Services (Master Node)

sudo systemctl start zookeeper  
$KAFKA_HOME/bin/kafka-server-start.sh $KAFKA_HOME/config/server.properties  
start-dfs.sh  
start-yarn.sh  
sudo systemctl start influxdb  
sudo systemctl start grafana-server  

---

### Kafka Topic Creation

$KAFKA_HOME/bin/kafka-topics.sh \
--create \
--topic airquality.raw \
--bootstrap-server master:9092 \
--replication-factor 2 \
--partitions 3

---

### Spark Structured Streaming (Kafka → HDFS)

cd ~/aqi_project/spark  
spark-submit kafka_to_hdfs.py  

This job continuously reads AQI data from Kafka and writes raw data to HDFS in Parquet format.

---

### AQI Producer (API → Kafka)

cd ~/aqi_project/producer  
source ~/aqi_project/venv/bin/activate  
python aqi_producer.py  

This script fetches AQI data for each city and publishes messages to Kafka.

---

### Spark Batch Job (HDFS → InfluxDB)

cd ~/aqi_project/spark  
spark-submit daily_kpi_to_influx.py  

This job computes daily average AQI per city and writes results to InfluxDB.

---

## Grafana Dashboard

Access Grafana via SSH tunneling:

ssh -L 3001:localhost:3000 user@master  

Open in browser:

http://localhost:3001  

Dashboard displays daily average AQI trends for Paris, Lyon, and Marseille.

---
