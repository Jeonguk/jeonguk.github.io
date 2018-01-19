---
layout: post
title:  "Redis Clustering"
date:   2018-01-19 18:34:10 +0700
categories: [redis, nosql]
---

> Redis Clustering setting

Redis Download
https://redis.io/download

### 1. Configure Redis Cluster master nodes

* Redis node master1 (redis-master1.conf)

```
port 6379
cluster-enabled yes
cluster-config-file nodes1.conf
cluster-node-timeout 5000
appendonly yes
```

* Redis node master2 (redis-master2.conf)

```
port 6380
cluster-enabled yes
cluster-config-file nodes2.conf
cluster-node-timeout 5000
appendonly yes
```

*  Redis node master3 (redis-master3.conf)

```
port 6381
cluster-enabled yes
cluster-config-file nodes3.conf
cluster-node-timeout 5000
appendonly yes
```
### 2. Redis server start

```
- redis-server redis-master1.conf
- redis-server redis-master2.conf
- redis-server redis-master3.conf
```

* 독립 실행 형 Redis 인스턴스의 콘솔 출력과 비교하면 두드러진 차이점이 있습니다.
* 시작시, 각 노드는 Redis Clustering in Nutshell에서 논의한 것처럼 고유 한 ID (이름)를 생성합니다.이 값은 최초 실행시에만 생성 된 다음 재사용됩니다
모든 인스턴스가 클러스터 모드에서 실행 중입니다.
또한 실행중인 모든 인스턴스에 대해 현재 노드 ID (이름)와 몇 가지 추가 정보로 만들어진 nodes.conf 파일이 있습니다
* 이 시점에서 클러스터 모드로 실행되는 3 개의 Redis 마스터 노드가 있지만 실제로 클러스터를 형성하지는 않습니다 
(모든 Redis 마스터 노드는 자체 만보고 다른 노드는 보이지 않습니다). 
이를 확인하기 위해 각 인스턴스에 대해 CLUSTER NODES 명령 (Redis Cluster Commands 섹션 참조)을 개별적으로 실행할 수 있으며 실제 상황을 관찰 할 수 있습니다.

``` 
 jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6379 CLUSTER NODES
d5cff78f514b6a647a2a226d603e5bf92034a2a6 :6379@16379 myself,master - 0 0 0 connected
 jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6380 CLUSTER NODES
9ce0eb4b5f3e3de992f3da657cc554d2ab7ecac7 :6380@16380 myself,master - 0 0 0 connected
 jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6381 CLUSTER NODES
4197e26072f913acb878745ed851505bc0218989 :6381@16381 myself,master - 0 0 0 connected
```

* 클러스터를 구성하려면 클러스터 모드에서 실행되는 Redis 노드를 CLUSTER MEET 명령으로 연결해야합니다 (Redis Cluster Commands 섹션 참조). 
유감스럽게도 명령은 IP 주소 만 허용하고 호스트 이름은 허용하지 않습니다. 
토폴로지에서 master1은 IP 주소 127.0.01 를 가지며 master2는 127.0.01  를 가지며 master3은  127.0.01 를 갖습니다.
 IP 주소가 있으면 master1 노드에 대해 명령을 내 보자.

```
  jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6379 CLUSTER MEET 127.0.0.1 6380
OK
 jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6379 CLUSTER MEET 127.0.0.1 6381
OK
```

```
jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6379 CLUSTER NODES
9ce0eb4b5f3e3de992f3da657cc554d2ab7ecac7 127.0.0.1:6380@16380 master - 0 1516335902583 1 connected
4197e26072f913acb878745ed851505bc0218989 127.0.0.1:6381@16381 master - 0 1516335903615 2 connected
d5cff78f514b6a647a2a226d603e5bf92034a2a6 127.0.0.1:6379@16379 myself,master - 0 1516335901000 0 connected
```
```
jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6380 CLUSTER NODES
d5cff78f514b6a647a2a226d603e5bf92034a2a6 127.0.0.1:6379@16379 master - 0 1516335919786 0 connected
9ce0eb4b5f3e3de992f3da657cc554d2ab7ecac7 127.0.0.1:6380@16380 myself,master - 0 1516335919000 1 connected
4197e26072f913acb878745ed851505bc0218989 127.0.0.1:6381@16381 master - 0 1516335920806 2 connected
```
```
jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6381 CLUSTER NODES
9ce0eb4b5f3e3de992f3da657cc554d2ab7ecac7 127.0.0.1:6380@16380 master - 0 1516335932676 1 connected
d5cff78f514b6a647a2a226d603e5bf92034a2a6 127.0.0.1:6379@16379 master - 0 1516335932155 0 connected
4197e26072f913acb878745ed851505bc0218989 127.0.0.1:6381@16381 myself,master - 0 1516335931000 2 connected
```

* line column info
Node ID (name)
IP : port of the node
Flags : master, slave, myself, fail...
If it is a slave, the Node ID (name) of the master
Time of the last pending PING still waiting for a reply
Time of the last PONG received
Configuration epoch for this node (see the please http://redis.io/topics/cluster-spec)
Status of the link to this node
Hash slots served

* 마지막 열인 Hash Slots가 출력에 설정되지 않았고 그 이유가 있습니다. 
우리는 아직 마스터 노드에 해시 슬롯을 할당하지 않았으므로 이것이 지금 할 일입니다.
 해시 슬롯은 특정 클러스터 노드에서 CLUSTER ADDSLOTS (Redis Cluster Commands를 참조하십시오) 
 명령을 사용하여 노드에 할당 될 수 있습니다 (각각 CLUSTER DELSLOTS를 사용하여 할당 해제됩니다). 
 불행히도 해시 슬롯 범위 (0-5400 등)를 할당 할 수는 없지만 대신 해시 슬롯 (총 16384 개 중)을 개별적으로 할당해야합니다.
 이 제한을 극복하는 가장 간단한 방법 중 하나는 쉘 스크립팅을 사용하는 것입니다. 
 클러스터에 3 개의 Redis 마스터 노드가 있으므로 16384 개의 해시 슬롯 범위를 다음과 같이 나눌 수 있습니다.

```
- Redis node master1 contains hash slots 0 – 5400
for slot in {0..5400}; do redis-cli -h 127.0.0.1 -p 6379 CLUSTER ADDSLOTS $slot; done;

- Redis node master2 contains hash slots 5401 – 10800
for slot in {5400..10800}; do redis-cli -h 127.0.0.1 -p 6380 CLUSTER ADDSLOTS $slot; done;

- Redis node master3 contains hash slots 10801 – 16383
for slot in {10801..16383}; do redis-cli -h 127.0.0.1 -p 6381 CLUSTER ADDSLOTS $slot; done;

* CLUSTER NODES 명령을 다시 한 번 실행하면 마지막 열은 각 마스터 노드에서 제공 한 적절한 해시 슬롯으로 채워집니다 
(이전에 노드에 할당 한 해시 슬롯 범위와 정확히 일치 함).
```

```
jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6379 CLUSTER NODES
9ce0eb4b5f3e3de992f3da657cc554d2ab7ecac7 127.0.0.1:6380@16380 master - 0 1516336497114 1 connected 5401-10800
4197e26072f913acb878745ed851505bc0218989 127.0.0.1:6381@16381 master - 0 1516336498148 2 connected 10801-16383
d5cff78f514b6a647a2a226d603e5bf92034a2a6 127.0.0.1:6379@16379 myself,master - 0 1516336496000 0 connected 0-5400
```

### 3. Configure Redis Cluster slave nodes and replication

* Redis 클러스터를 완성하려면 각 실행중인 Redis 마스터 노드에 정확하게 하나의 슬레이브 노드를 추가해야합니다. 
이 튜토리얼의 Part 3 인 Redis Replication은 복제 구성을 충분히 다루고 있지만 Redis 클러스터는 복제 구성을 다르게 처리합니다. 
처음부터 슬레이브를 실행하고 구성하는 절차는 마스터와 다르지 않습니다 (유일한 차이점은 포트 번호입니다).

- Redis node slave1 (redis- slave1.conf)
```
port 7379
cluster-enabled yes
cluster-config-file nodes-slave1.conf
cluster-node-timeout 5000
appendonly yes
```
- Redis node slave2 (redis- slave2.conf)
```
port 7380
cluster-enabled yes
cluster-config-file nodes-slave1.conf
cluster-node-timeout 5000
appendonly yes
```
- Redis node slave3 (redis- slave3.conf)
```
port 7331
cluster-enabled yes
cluster-config-file nodes-slave1.conf
cluster-node-timeout 5000
appendonly yes
```
- Start slave server
```
redis-server redis-slave1.conf
redis-server redis-slave2.conf
redis-server redis-slave3.conf
```

* CLUSTER MEET에 IP 주소가 필요하므로 
slave1의 IP 주소는 127.0.0.1 이고, 
slave2의 IP 주소는 127.0.0.1  이며 
slave3의 IP 주소는 127.0.0.1  입니다.

```
redis-cli -h 127.0.0.1 -p 6379 CLUSTER MEET 127.0.0.1 7379
redis-cli -h 127.0.0.1 -p 6379 CLUSTER MEET 127.0.0.1 7380
redis-cli -h 127.0.0.1 -p 6379 CLUSTER MEET 127.0.0.1 7381
```
```
 jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6379 CLUSTER MEET 127.0.0.1 7379
OK
 jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6379 CLUSTER MEET 127.0.0.1 7380
OK
 jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6379 CLUSTER MEET 127.0.0.1 7381
OK
```

* 항상 CLUSTER NODES 명령을 사용하여 Redis 클러스터의 현재 노드를 볼 수 있습니다 (총 6 개). 
출력은 모든 노드를 마스터로 표시합니다.

```
 jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6379 CLUSTER NODES
9fc370ad7bd5021f67f0c6ed006fa81fa00e389f 127.0.0.1:7379@17379 master - 0 1516337158847 3 connected
9ce0eb4b5f3e3de992f3da657cc554d2ab7ecac7 127.0.0.1:6380@16380 master - 0 1516337159000 1 connected 5401-10800
d5cff78f514b6a647a2a226d603e5bf92034a2a6 127.0.0.1:6379@16379 myself,master - 0 1516337159000 4 connected 0-5400
4197e26072f913acb878745ed851505bc0218989 127.0.0.1:6381@16381 master - 0 1516337159878 2 connected 10801-16383
306f7ae7cf80ff63b7e71de480ce3b2d5fb3feb9 127.0.0.1:7381@17381 master - 0 1516337159568 5 connected
dfb316d637ff84e6bf2e15169f5c6f13ab6aa849 127.0.0.1:7380@17380 master - 0 1516337159000 0 connected
```

* 복제를 구성하려면 마스터 노드 ID (name)를 제공하여 각 Redis 슬레이브에서 새로운 CLUSTER REPLICATE 명령을 실행해야합니다. 
다음 테이블은 (CLUSTER NODES 명령 결과의 결과를 참조하여) 복제에 필요한 모든 부분을 함께 요약합니다.

```
- redis-cli -h 127.0.0.1 -p 7379 CLUSTER REPLICATE d5cff78f514b6a647a2a226d603e5bf92034a2a6
- redis-cli -h 127.0.0.1 -p 7380 CLUSTER REPLICATE 9ce0eb4b5f3e3de992f3da657cc554d2ab7ecac7
- redis-cli -h 127.0.0.1 -p 7381 CLUSTER REPLICATE 4197e26072f913acb878745ed851505bc0218989
```

* 이 시점에서 Redis 클러스터는 올바르게 구성되었으며 우리가 의도 한 토폴로지를가집니다. 
CLUSTER NODES 명령은 마스터에 연결된 모든 슬레이브를 표시합니다.

```
 jeonguk@JEONGUKui-MacBook-Pro  /usr/local/etc  redis-cli -h 127.0.0.1 -p 6379 CLUSTER NODES
9fc370ad7bd5021f67f0c6ed006fa81fa00e389f 127.0.0.1:7379@17379 slave d5cff78f514b6a647a2a226d603e5bf92034a2a6 0 1516337431297 4 connected
9ce0eb4b5f3e3de992f3da657cc554d2ab7ecac7 127.0.0.1:6380@16380 master - 0 1516337431000 1 connected 5401-10800
d5cff78f514b6a647a2a226d603e5bf92034a2a6 127.0.0.1:6379@16379 myself,master - 0 1516337431000 4 connected 0-5400
4197e26072f913acb878745ed851505bc0218989 127.0.0.1:6381@16381 master - 0 1516337431000 2 connected 10801-16383
306f7ae7cf80ff63b7e71de480ce3b2d5fb3feb9 127.0.0.1:7381@17381 slave 4197e26072f913acb878745ed851505bc0218989 0 1516337432319 5 connected
dfb316d637ff84e6bf2e15169f5c6f13ab6aa849 127.0.0.1:7380@17380 slave 9ce0eb4b5f3e3de992f3da657cc554d2ab7ecac7 0 1516337432116 1 connected
```

### 4. Redis 클러스터가 올바르게 작동하는지 확인

* Redis의 경우 항상 그렇듯이 Redis 클러스터를 예상대로 작동시키는 가장 좋은 방법은 redis-cli를 사용하여 몇 가지 명령을 실행하는 것입니다. 
클러스터의 노드는 명령을 프록시하지 않고 클라이언트를 대신 리디렉션하므로 (Sharding (Partitioning) Scheme 참조) 클라이언트는 이러한 프로토콜을 
지원해야하므로 redis-cli가 -c 명령 줄 옵션과 함께 실행되어야합니다 ( 클러스터 지원 포함) :

```
- redis-cli -h master1 -p 6379 -c
```

* 저장된 키를 SET 명령을 사용하여 설정하고 나중에 쿼리 할 수 ​​있습니다 (GET 명령 사용). 
세 개의 노드 사이에 해시 슬롯을 분산 시켰기 때문에 키는 모든 노드에 분산됩니다. 
이름이 some-key 인 첫 번째 키는 우리가 연결되어있는 master1 노드 자체에 저장됩니다.

```
127.0.0.1:6379> SET some-key some-value
OK
127.0.0.1:6379> GET some-key
"some-value"
```

* 그러나 키를 다른 키로 저장하려고하면 재미있는 일이 일어날 것입니다 : 
redis-cli는 IP 주소가 127.0.0.1 (master3) 인 노드에 값이 저장 될 것이라고 말합니다. 
이 키가 속한 해시 슬롯을 보유합니다.

```
127.0.0.1:6379> SET some-another-key some-value-another-value
-> Redirected to slot [15929] located at 127.0.0.1:6381
OK
127.0.0.1:6381> GET some-another-key
"some-value-another-value"
```

* 명령 실행 후 redis-cli는 자동으로 노드 127.0.0.1 (master3)로 리디렉션됩니다. 
클러스터 노드 127.0.0.1 (master3)에있게되면 CLUSTER GETKEYSINSLOT 명령을 실행하여 해시 슬롯에 실제로 다른 키가 포함되어 있는지 확인할 수 있습니다.
```
127.0.0.1:6381> CLUSTER GETKEYSINSLOT 15929 1
1) "some-another-key"
```
* 우리는 또한 Redis 종속 노드 slave3이 master (master3)에서 key some-another-key를 복제하고 그 값을 반환하는지 확인할 수 있습니다.


### Redis Sentinel

* Redis의 또 다른 위대하지만 여전히 실험적인 기능은 Redis Sentinel입니다. 
다음 목표를 염두에두고 라이브 Redis 인스턴스를 관리 할 수 ​​있도록 설계된 시스템입니다.

- 모니터링 : Sentinel은 마스터 및 슬레이브 인스턴스가 예상대로 작동하는지 지속적으로 확인합니다.
- 알림 : Sentinel은 모니터 된 Redis 인스턴스 중 하나에 문제가있을 경우이를 알릴 수 있습니다
- 자동 페일 오버 : 일부 마스터 노드가 예상대로 작동하지 않는 경우 Sentinel은 페일 오버 프로세스를 시작할 수 있습니다.
이 페일 오버 프로세스 중 하나는 마스터로 승격됩니다

