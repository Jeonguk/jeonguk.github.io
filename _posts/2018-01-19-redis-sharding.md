---
layout: post
title:  "Redis Sharding"
date:   2018-01-19 19:34:10 +0700
categories: [redis, nosql]
---

## Redis Sharding

* 우리가 다루고있는 데이터의 양은 매일 매일 기하 급수적으로 증가하고 있습니다. 
필요한 데이터를 메모리에 저장할 수 없으며 실제 스토리지가 더 이상 충분하지 않은 경우 종종 단일 상자에 대한 하드웨어 제한이 있습니다. 
수년에 걸쳐 이러한 문제로 인해 업계는 그러한 한계를 극복 할 수있는 데이터 샤딩 (또는 데이터 파티셔닝) 솔루션을 개발했습니다.
Redis에서 데이터 분할 (파티셔닝)은 모든 인스턴스가 키의 하위 집합 만 포함하도록 여러 Redis 인스턴스간에 모든 데이터를 분할하는 기술입니다. 
이러한 프로세스를 통해 점점 더 많은 인스턴스를 추가하고 더 작은 부분 (파편 또는 파티션)으로 데이터를 분할함으로써 증가하는 데이터를 완화 할 수 있습니다. 
뿐만 아니라, 수평 적 스케일링을 효과적으로 지원하면서 데이터를 처리하는 데 더 많은 연산 능력을 사용할 수 있음을 의미합니다.
모든 것이 윈 - 윈 (win-win) 솔루션이긴하지만 고려해야 할 절충안이 있습니다. 
여러 인스턴스로 데이터를 분할함으로써 특정 키 (또는 키)를 찾는 문제가 문제가됩니다. 
그것이 sharding (partitioning) scheme이 그림이되는 곳입니다 : 
일관되거나 고정 된 규칙에 따라 데이터를 분할 (분할)해야하므로 동일한 키에 대한 쓰기 및 읽기 작업이이 키를 보유하는 Redis 인스턴스로 이동해야합니다

```
- wget http://twemproxy.googlecode.com/files/nutcracker-0.3.0.tar.gz
- tar xfz nutcracker-0.3.0.tar.gz
- cd nutcracker-0.3.0
- ./configure
- make
- sudo make install
```

- range partitioning
- hash partitioning
- consistent hashing


* 기본적으로 twemproxy (nutcracker)는 / usr / local / sbin / nutcracker에 있습니다. 설치가 이루어지면 가장 중요한 부분은 구성입니다.
* Twemproxy (nutcracker)는 YAML을 구성 파일 형식으로 사용합니다 
(http://www.yaml.org/). twemproxy (nutcracker)가 지원하는 많은 설정 중에서 샤딩 (파티셔닝)과 관련된 설정을 선택합니다.

* twemproxy (nutcracker) 배포판의 conf / nutcracker.yml 파일은 다른 구성 예제를 찾는 좋은 시작입니다. 
시연에 대해서는 위에 표시된 토폴로지를 반영하여 샤드 된 서버 풀을 다음과 같이 시작합니다.

```
sharded:
    listen: 127.0.0.1:22122
    hash: fnv1a_64
    distribution: ketama
    auto_eject_hosts: true
    redis: true
    server_retry_timeout: 2000
    server_failure_limit: 2
    servers:
       - 127.0.0.1:6380:1
       - 127.0.0.1:6381:1
       - 127.0.0.1:6382:1
```

* 샤드 된 서버 풀은 키 분배기를 fnv1a_64로 설정하여 키 분배에 키타 마 일관성 해시를 사용합니다.
* twemproxy (nutcracker)를 시작하기 전에 포트 6380, 6381 및 6382에서 세 개의 Redis 인스턴스를 모두 실행해야합니다.
```
redis-server --port 6380
redis-server --port 6381
redis-server --port 6382
```

* 그 후 샘플 구성을 사용하여 twemproxy (nutcracker)의 인스턴스를 다음 명령을 사용하여 시작할 수 있습니다.
nutcracker -c nutcracker-sharded.yml

- TEST
```
jeonguk@JEONGUKui-MacBook-Pro  ~  redis-cli -p 22122
127.0.0.1:22122> SET userkey uservalue
OK
127.0.0.1:22122> SET somekey somevalue
OK
127.0.0.1:22122> SET anotherkey anothervalue
OK
127.0.0.1:22122> MGET userkey somekey anotherkey
1) "uservalue"
2) "somevalue"
3) "anothervalue"
127.0.0.1:22122>
```
```
jeonguk@JEONGUKui-MacBook-Pro  ~  redis-cli -p 6380 MGET userkey somekey anotherkey
1) (nil)
2) (nil)
3) (nil)
```
```
jeonguk@JEONGUKui-MacBook-Pro  ~  redis-cli -p 6381 MGET userkey somekey anotherkey
1) "uservalue"
2) (nil)
3) (nil)
```
```
jeonguk@JEONGUKui-MacBook-Pro  ~  redis-cli -p 6382 MGET userkey somekey anotherkey
1) (nil)
2) "somevalue"
3) "anothervalue"
```

* 흥미로운 질문이 제기 될 수 있습니다 : 키가 왜 이렇게 저장됩니까? 대답은 구성된 해시 함수입니다. 
키는 서버 풀의 모든 Redis 인스턴스에 일관되게 분산됩니다. 
그러나 균형 잡힌 (짝수 또는 임의의) 분포를 가지기 위해 구성된 해쉬 함수는 애플리케이션에서 사용되는 키 네이밍 패턴과 관련하여 매우 신중하게 선택되어야합니다. 
예에서 알 수 있듯이 키는 모든 인스턴스에 균등하게 분배되지 않습니다 
(첫 번째 인스턴스에는 아무 것도 없으며 두 번째 키에는 하나의 키가 있고 세 번째 키에는 두 개의 키가 있음).
마지막주의 사항 : twemproxy (nutcracker)는 Redis 프로토콜을 지원하지만, 
Sharding (Partitioning) 섹션을 사용할 때의 제한 사항으로 인해 모든 명령이 지원되는 것은 아닙니다.
twemproxy (nutcracker)에 대한 자세한 내용은 https://github.com/twitter/twemproxy를 참조하십시오. 최신 문서가 제공됩니다.
