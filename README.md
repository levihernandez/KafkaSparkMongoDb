# Monitor Kafka/Spark/MongoDB Containers with Datadog

The goal of the current project is to show the capabilities of Datadog solutions to monitor Docker containers and internal processes. In the second phase of the project, we aim to trace the application actions starting by collecting Kafka topic data with Spark Streaming and store the data into MongoDb. 

## Docker Images

> Pull Datadog, Kafka, Spark, MongoDb from Docker hub

```bash
$ docker pull datadog/agent:latest
$ docker pull mongo:latest
$ docker pull bitnami/kafka
$ docker pull bitnami/spark
```

## Unify Containers with Docker Compose

Inspect the `docker-compose.yml` in this repo to see how all four containers are connected. Replace the `- DD_API_KEY=<DATADOG-API-KEY>` with your valid Datadog API key and run `$ docker-compose up -d` to start the containers. Log into Datadog UI to preview the Network, Infra, Container metrics.

## Kafka

In progress...

## Spark



## MongoDB

Test the mongo server connection:

```bash
$ docker exec -it mongodb-container bash

root@0356dd52f470:/# mongo
MongoDB shell version v4.4.1
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("d8eb2172-7efa-45df-b3d0-55581fe5c843") }
MongoDB server version: 4.4.1
Welcome to the MongoDB shell.
```


## Tracing Spark Streaming

Goals:

* Spark Streaming task collects Kafka Topics & Metadata to create a custom `kafka_id`
* Trace Spark application with Datadog APM tracing libraries
* Instrument Spark code and pass the `kafka_id` to the tracing spans
* Present the `kafka_id` in Datadog's APM flamegraph