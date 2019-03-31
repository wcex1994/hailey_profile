# Kafka

Kafka is an open-source stream app for real-time data pipeline.

* Timeline: decoupling the time-sensitivity requirement between producers and consumers
* Reliability: at-least-once delivery
* High and Varying Throughput: scalable by adding consumer and producer independently
* Data Format: agnostic to data format
* Transformation: ETL and ELT
* Security: Encryption
* Failure Handling: keeping for longer period
* Coupling and Agility: ad-hoc pipeline; loss of metadata; extreme processing

## Kafka Connect

Kafka Connect is a part of Apache Kafka and provides a scalable and reliable way to move data between Kafka and other datastores.

* Scalable
* Connector Plugins
* Convertor

### Running Connect

```bash
bin/connect-distributed.sh config/connect-distributed.properties
```

* bootstrap.servers:: Kafka broker that Connect will work with. Pipe the data to or from those brokers.
* group.id:: All workers with the same group ID are part of the same Connect cluster.
* key.converter and value.converter:: Default is JSON format.

```bash
# check which connector plugins are available
curl http://localhost:8083/connector-plugins

# pipe config into a topic 
echo '{"name":"load-kafka-config", "config":{"connector.class":
"FileStreamSource","file":"config/server.properties","topic":
"kafka-config-topic"}}' | curl -X POST -d @- http://localhost:8083/connectors
--header "content-Type:application/json"

# use Console to check if we have loaded the config into a topic
bin/kafka-console-consumer.sh --new-consumer --bootstrap-server=localhost:9092 --topic kafka-config-topic --from-beginning

# delete a connector 
curl -X DELETE http://localhost:8083/connectors/dump-kafka-config
```

Kafka Connector includes two parts, *connectors* and *tasks*. The connector is responsbile for 1) Determining how many tasks will run for the connector 1) Deciding how to split the data-copying work between the tasks 3) Getting configurations for the tasks from the workers and passing it along. Task is responsible for actually getting the data i nand out of Kafka. Kafka Connect's worker processes are the container processes that execute the connectors and tasks. The best way to understand workers is to realize that connectors and tasks are responsible for the “moving data” part of data integration, while the workers are responsible for the REST API, configuration management, reliability, high availability, scaling, and load balancing. This seperation is a benefit from using Connect API.




## Source

* <https://learning.oreilly.com/library/view/kafka-the-definitive/9781491936153/ch07.html>