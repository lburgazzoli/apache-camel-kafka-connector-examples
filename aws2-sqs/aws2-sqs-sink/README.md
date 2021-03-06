# Camel-Kafka-connector AWS2 SQS Sink

## Introduction

This is an example for Camel-Kafka-connector AW2-SQS Sink 

## What is needed

- An AWS SQS queue

## Running Kafka

```
$KAFKA_HOME/bin/zookeeper-server-start.sh config/zookeeper.properties
$KAFKA_HOME/bin/kafka-server-start.sh config/server.properties
$KAFKA_HOME/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic mytopic
```

## Setting up the needed bits and running the example

You'll need to setup the plugin.path property in your kafka

Open the `$KAFKA_HOME/config/connect-standalone.properties`

and set the `plugin.path` property to your choosen location

In this example we'll use `/home/oscerd/connectors/`

```
> cd /home/oscerd/connectors/
> wget https://repo1.maven.org/maven2/org/apache/camel/kafkaconnector/camel-aws2-sqs-kafka-connector/0.2.0/camel-aws2-sqs-kafka-connector-0.2.0-package.zip
> unzip camel-aws2-sqs-kafka-connector-0.2.0-package.zip
```

Now it's time to setup the connectors

Open the AWS2 SQS configuration file

```
name=CamelAWS2SQSSinkConnector
connector.class=org.apache.camel.kafkaconnector.aws2sqs.CamelAws2sqsSinkConnector
key.converter=org.apache.kafka.connect.storage.StringConverter
value.converter=org.apache.kafka.connect.storage.StringConverter

topics=mytopic

camel.sink.path.queueNameOrArn=camel-1

camel.component.aws2-sqs.access-key=xxxx
camel.component.aws2-sqs.secret-key=yyyy
camel.component.aws2-sqs.region=eu-west-1
```

and add the correct credentials for AWS.

Now you can run the example

```
$KAFKA_HOME/bin/connect-standalone.sh $KAFKA_HOME/config/connect-standalone.properties config/CamelAWS2SQSSinkConnector.properties
```

Just connect to your AWS Console and poll message on the SQS Queue Camel-1

On a different terminal run the kafka-producer and send messages to your Kafka Broker.

```
bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic mytopic
Kafka to SQS message 1
Kafka to SQS message 2
```

You shold see the messages enqueued in the camel-1 SQS queue.

