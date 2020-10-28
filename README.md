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

## Datadog Container

In order to properly see Network metrics in the DD UI, an additional mount is required inside the Datadog Docker container `root@ddagentcontainer:/var/log/datadog# mount -t debugfs none /sys/kernel/debug`

## Kafka

In progress...

## Spark

Run an example test to validate that Datadog UI is receiving data in the [containers](https://app.datadoghq.com/containers) section.

```bash
$ docker exec -it spark-container bash
@sparkcontainer:/opt/bitnami/spark$ spark-submit --master local[*] --name HdfsWordCount examples/src/main/python/pi.py
20/10/28 00:01:53 INFO DAGScheduler: Job 0 finished: reduce at /opt/bitnami/spark/examples/src/main/python/pi.py:44, took 1.670663 s
Pi is roughly 3.134240
```

* Next step: configure Spark metrics after enabling the Datadog [Spark Integration](https://app.datadoghq.com/account/settings#integrations/spark)

For now you can observe the Docker containers receiving data in a real-time form.

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

* Next Step: enable the Datadog [Mongo Integration](https://app.datadoghq.com/account/settings#integrations/mongodb)

## Tracing Spark Streaming

Goals:

* Spark Streaming task collects Kafka Topics & Metadata to create a custom `kafka_id`
* Trace Spark application with Datadog APM tracing libraries
* Instrument Spark code and pass the `kafka_id` to the tracing spans
* Present the `kafka_id` in Datadog's APM flamegraph
