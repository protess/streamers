[![Build Status](https://travis-ci.org/samaitra/streamers.svg?branch=master)](https://travis-ci.org/samaitra/streamers)

# streamers
A collection of streaming application

### Stream processing topology

![Stream processing topology](https://github.com/samaitra/streamers/raw/master/resources/streamers.png) 

### Setup: Flink 다운로드 받기

(https://flink.apache.org/downloads.html)
위에서 flink 바이너리를 다운로드 받습니다. 마음에 드는 최신 버전으로 받으시면 됩니다.

### 다운로드 받은 바이너리 압축풀기 
```
$ cd ~/Downloads        # Go to download directory
$ tar xzf flink-*.tgz   # Unpack the downloaded archive
$ cd flink-1.6.1
```

### 로컬에서 flink 클러스터 실행하기
```
$ ./bin/start-cluster.sh  # Start Flink
```
웹브라우저에서 http://localhost:8081 로 flink 웹 인터페이스를 사용 하실 수 있습니다 여기서 태스크 매니저가 하나 있는것을 보실 수 있습니다

### Dispatcher: Overview

![Flink Dispatcher Overview](https://github.com/samaitra/streamers/raw/master/resources/flink_job.png) 

로그파일로 실행여부를 확인 할 수 있습니다:
```
$ tail log/flink-*-standalonesession-*.log
```

### 카프카 다운로드

(https://kafka.apache.org/downloads).
위에서 카프카를 다운로드 받습니다 최신버전으로 받아 주세요

### zookeeper 를 먼저 실행 
```
$./bin/zookeeper-server-start.sh ./config/zookeeper.properties
```

### 카프카 브로커 실행 
```
./bin/kafka-server-start.sh ./config/server.properties 
```

### “mytopic” 생성
```
$ ./bin/kafka-topics.sh --create --topic mytopic --zookeeper localhost:2181 --partitions 1 --replication-factor 1
```

### "mytopic" 설명

```
./bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic mytopic
```

### 콘솔 프로듀서를 실행해서 아무 문자나 입력하고 엔터 를 눌러서 토픽에 넣기
```
./bin/kafka-console-producer.sh --broker-list localhost:9092 --topic mytopic
```

### 콘솔 컨슈머를 실행하기 
```
./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic mytopic --from-beginning
```

### 아파치 이그나이트를 깃헙에서 클론 받기

플링크 클러스터에서 IgniteSink 를 사용하려면 2.7.0 버전에서 마스터 브렌치를 받아야합니다
 

```
$ git clone https://github.com/apache/ignite
```

### 아파치 이그나이트 빌드: 

```
$ mvn clean package install -DskipTests
```


### 플링크 프로젝트 빌드: 
```
$ mvn clean package
```

### 플링크 프로그램 실행 :
```
$ ./bin/flink run streamers-1.0-SNAPSHOT.jar
```

### 콘솔 프로듀서에서 새로운 문자 입력후 엔터
```
$ ./bin/kafka-console-producer.sh --topic mytopic --broker-list localhost:9092
```

The .out file will print the counts at the end of each time window as long as words are floating in, e.g.:
```
$ tail -f log/flink-*-taskexecutor-*.out
lorem : 1
bye : 1
ipsum : 4
```

### 이그나이트 REST 서비스 
캐쉬 키/벨류 보기 

```
$ curl -X GET http://localhost:8080/ignite\?cmd\=getall\&k1\=jam\&cacheName\=testCache
```

### 캐쉬 스캔
To check all the keys from an Ignite cache the following rest service can be used
```
$ curl -X GET http://localhost:8080/ignite?cmd=qryscanexe&pageSize=10&cacheName=testCache
```

### 이그나이트 웹 콘솔
그리드게인 웹 콘솔 서비스(https://console.gridgain.com/)


### Flink 클러스터 정지 시키기:
```
$ ./bin/stop-cluster.sh
```