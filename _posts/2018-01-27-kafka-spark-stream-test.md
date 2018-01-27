---
layout: post
title:  "Kafka Spark streaming test"
date:   2018-01-27 22:22:10 +0700
categories: [spark,flume,kafka,bigdata]
---

# 작성중 ....

# SPARK


### spark download

* https://www.apache.org/dyn/closer.lua/spark/spark-2.2.1/spark-2.2.1-bin-hadoop2.7.tgz

### configure

```
export SPARK_HOME= <PATH>
export PATH=$PATH:$SPARK_HOME/bin
```

# Submit our Spark Streaming Application

```
$ cd <PROJECT_HOME>/target
$ spark-submit --class com.jeonguk.spark.StreamingKafkaDirectStringDecoder --deploy-mode client kafka-sparkstream-0.0.1-SNAPSHOT-jar-with-dependencies.jar localhost:9092
```

# Launch the Flume Agent using the Twitter Source and Kafka Channel

```
$ cd $FLUME_HOME
$ bin/flume-ng agent --name FlumeKafkaAgent --conf conf --conf-file conf/flume-kafka.conf
```