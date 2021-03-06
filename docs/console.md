# Kafka console commands
## Start Zookeeper
Kafka uses [ZooKeeper](https://zookeeper.apache.org/) so you need to first start a ZooKeeper server if you don't already have one.
```sh
> bin/zookeeper-server-start.sh config/zookeeper.properties
```

## Start Kafka Broker
```sh
> bin/kafka-server-start.sh config/server.properties
```

## Create a topic
Let's create a topic named "test" with a single partition and only one replica:
1
```sh
> bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
```

We can now see that topic if we run the list topic command:
```sh
> bin/kafka-topics.sh --list --zookeeper localhost:2181
test
```

Now check the details of kafka topic with the command:
```sh
> bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic test
```
Alternatively, instead of manually creating topics you can also configure your brokers to auto-create topics when a non-existent topic is published to.

## Send messages using the console producer
Kafka comes with a command line client that will take input from a file or from standard input and send it out as messages to the Kafka cluster. By default, each line will be sent as a separate message.

Run the producer and then type a few messages into the console to send to the server.
```sh
> bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
This is a message
This is another message
```

## Start a consumer
Kafka also has a command line consumer that will dump out messages to standard output.
```sh
> bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
This is a message
This is another message
```

If you have each of the above commands running in a different terminal then you should now be able to type messages into the producer terminal and see them appear in the consumer terminal. 

#### Start a consumer as part of a consumer group
Add the following command to the end of the previous command.
```sh
--consumer-property group.id=your_group
```

### Alter partitions of a topic
```shell script
> bin/kafka-topics.sh --alter --zookeeper localhost:2181 --topic test-1 --partitions 3
```

### Describe a consumer group
```shell script
> bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group your_group
```