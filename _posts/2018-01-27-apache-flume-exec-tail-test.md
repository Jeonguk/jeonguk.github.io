## Apache Flume info 

```
http://flume.apache.org/FlumeUserGuide.html
```

``` Apache Flume Download ```

```
http://www.apache.org/dyn/closer.lua/flume/1.8.0/apache-flume-1.8.0-bin.tar.gz
```

``` Configuration File ```

> apache-flume-1.8.0-bin/conf

* apache-log.config
```
LogAgent.sources = source1
LogAgent.sinks = sink1
LogAgent.channels = channel1

LogAgent.sources.source1.type = exec
LogAgent.sources.source1.command = tail -F <Your Path>/fake-logger/fakelog.log
LogAgent.sources.source1.channels = channel1

LogAgent.sinks.sink1.type = logger

LogAgent.channels.channel1.type = memory
LogAgent.channels.channel1.capacity = 1000
LogAgent.channels.channel1.transactionCapacity = 100

LogAgent.sources.source1.channels = channel1
LogAgent.sinks.sink1.channel = channel1

```

``` RUN TEST ```

```
./bin/flume-ng agent -c conf -f ./conf/apache-log.config --name LogAgent -Dflume.root.logger=DEBUG,console -Xmx512m -Xms100m
```


``` Fake Logger generator ```
* /fake-logger/fakelog.log
```
https://github.com/bitsofinfo/log-generator
```
