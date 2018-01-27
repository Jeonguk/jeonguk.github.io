---
layout: post
title:  "Flume - Kafka pipeline test"
date:   2018-01-27 13:53:10 +0700
categories: [bigdata]
---

## Apache Kafka

### Kafka info

* https://kafka.apache.org/quickstart

``` Download & Install ```

* https://www.apache.org/dyn/closer.cgi?path=/kafka/1.0.0/kafka_2.11-1.0.0.tgz

``` Basic Configuration of the internal ZooKeeper Service ```
```
vi config/zookeeper.properties
```

``` Absolute path to the ZooKeeper Data Directory and Client Port ```
```
dataDir=<ZooKeeper Data Directory>
clientPort=2181
```

``` Basic Configuration of the Kafka Server ```
```
vi config/server.properties
```

``` Kafka Server Log Directory and ZooKeeper Co-ordinator Service ```
```
log.dirs=<Kafka Log Directory>
zookeeper.connect=<Hostname>:2181
```

``` Start the internal ZooKeeper Service ```
```
$ bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
```

``` Start the Kafka Server ```
```
$ bin/kafka-server-start.sh config/server.properties
```

``` Create the Log Topic ```
```
$ bin/kafka-topics.sh --create --zookeeper <ZooKeeper Hostname>:2181 --replication-factor 1 --partitions 2 --topic log-topic
```
Created topic "log-topic".

``` Run a Kafka Consumer subscribed to the log-topic Topic ```
```
cd $KAFKA_HOME
bin/kafka-console-consumer.sh --zookeeper <ZooKeeper Hostname>:2181 --topic log-topic --from-beginning
```

## Apache Flume info 

```
http://flume.apache.org/FlumeUserGuide.html
```

``` Flume configuration ```

* conf/flume-kafka.config

```
# The configuration file needs to define the sources,
# the channels and the sinks.
# Sources, channels and sinks are defined per agent,
# in this case called 'FlumeKafkaAgent'

# Flume Instance - Flume Kafka Agent
FlumeKafkaAgent.sources = LogSource
FlumeKafkaAgent.channels = MemoryChannel
FlumeKafkaAgent.sinks = KafkaSink

# Source Configuration - Inbuilt LogSource
FlumeKafkaAgent.sources.LogSource.type = exec
FlumeKafkaAgent.sources.LogSource.command = tail -F /Users/jeonguk/PROJECT/TEST/fake-logger/fakelog.log
FlumeKafkaAgent.sources.LogSource.restart = true
FlumeKafkaAgent.sources.LogSource.batchSize = 1000

# Describe the sink
FlumeKafkaAgent.sinks.KafkaSink.type = org.apache.flume.sink.kafka.KafkaSink
FlumeKafkaAgent.sinks.KafkaSink.brokerList = localhost:9092
FlumeKafkaAgent.sinks.KafkaSink.topic = log-topic

# Channel Configuration - MemoryChannel
# Use a channel which buffers events in memory
FlumeKafkaAgent.channels.MemoryChannel.type = memory
FlumeKafkaAgent.channels.MemoryChannel.capacity = 1000
FlumeKafkaAgent.channels.MemoryChannel.transactionCapacity = 100

# Bind the source and sink to the channel
FlumeKafkaAgent.sources.LogSource.channels = MemoryChannel
FlumeKafkaAgent.sinks.KafkaSink.channel = MemoryChannel
```


``` Launch the Flume Agent using the Kafka Channel and sinking to the Console ```
```
bin/flume-ng agent --name FlumeKafkaAgent --conf conf --conf-file conf/flume-kafka.conf -Dflume.root.logger=DEBUG,console
```
