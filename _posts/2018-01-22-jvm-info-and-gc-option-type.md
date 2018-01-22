---
layout: post
title:  "Java JVM & Garbage Collection Type"
date:   2018-01-22 13:53:10 +0700
categories: [java, language]
---

### Java JVM & Garbage Collection Type

* JVM Options Info
 
 ```
 jeonguk@JEONGUKui-MacBook-Pro  ~  java -XX:+PrintCommandLineFlags -version
-XX:InitialHeapSize=268435456 -XX:MaxHeapSize=4294967296 -XX:+PrintCommandLineFlags -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:+UseParallelGC
java version "1.8.0_151"
Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
 jeonguk@JEONGUKui-MacBook-Pro  ~ 
```

* Available Options by GC Type.

```
Serial GC : 
    -XX:+UseSerialGC
Parallel GC : 
    -XX:+UseParallelGC
    -XX:ParallelGCThreads=value
Parallel Compacting GC : 
    -XX:+UseParallelOldGC
CMS GC :
    -XX:+UseConcMarkSweepGC
    -XX:+UseParNewGC
    -XX:+CMSParallelRemarkEnabled
    -XX:CMSInitiatingOccupancyFraction=value
    -XX:+UseCMSInitiatingOccupancyOnly
G1 (In JDK 6, these two options must be used together.)
    -XX:+UnlockExperimentalVMOptions
    -XX:+UseG1GC
```

