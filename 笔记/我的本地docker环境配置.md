# 我的本地docker环境配置

## Kafka

```dockerfile
//拉取镜像
docker pull wurstmeister/kafka
//启动镜像
//mac
docker run -d --name kafka --publish 9092:9092 --link zookeeper --env KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 --env KAFKA_ADVERTISED_HOST_NAME=192.168.59.101 --env KAFKA_ADVERTISED_PORT=9092 --volume /etc/localtime:/etc/localtime wurstmeister/kafka:latest

docker run -d --name kafka_Test -p 9092:9092 -e KAFKA_BROKER_ID=0 -e KAFKA_ZOOKEEPER_CONNECT=172.17.0.3:2181/kafka -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092 -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 -v /etc/localtime:/etc/localtime wurstmeister/kafka

//windows
docker run -d --name kafka_Test -p 9092:9092 -e KAFKA_BROKER_ID=0 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092 -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 -e KAFKA_ZOOKEEPER_CONNECT=host.docker.internal:2181 -e KAFKA_ADVERTISED_HOST_NAME=192.168.59.101 -e KAFKA_ADVERTISED_PORT=9092 -v /etc/localtime:/etc/localtime wurstmeister/kafka

docker run -d --name kafka_Test -p 9092:9092 -e KAFKA_BROKER_ID=0 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092 -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 -e KAFKA_ZOOKEEPER_CONNECT=host.docker.internal:2181 -e KAFKA_ADVERTISED_PORT=9092 -v /etc/localtime:/etc/localtime wurstmeister/kafka
```

## Zookeeper

```dockerfile
docker pull wurstmeister/zookeeper
  
docker run -d --name zookeeper -p 2181:2181 -t wurstmeister/zookeeper


docker run -d --name zookeeper -p 2181:2181 -v /etc/localtime:/etc/localtime wurstmeister/zookeeper
```

## 启动nacos镜像(mac系统 m1芯片特殊镜像)

```dockerfile
docker run --name nacos-quick -e MODE=standalone -e MYSQL_SERVICE_HOST=host.docker.internal -e MYSQL_SERVICE_PORT=3306 -e MYSQL_SERVICE_DB_NAME=nacos -e MYSQL_SERVICE_USER=root -e MYSQL_SERVICE_PASSWORD=123456 -e SPRING_DATASOURCE_PLATFORM=mysql -p 8848:8848 -d zill057/nacos-server-apple-silicon:2.0.3


docker run --name nacos-quick -e MODE=standalone -e MYSQL_SERVICE_HOST=host.docker.internal -e MYSQL_SERVICE_PORT=3306 -e MYSQL_SERVICE_DB_NAME=nacos -e MYSQL_SERVICE_USER=root -e MYSQL_SERVICE_PASSWORD=123456 -e SPRING_DATASOURCE_PLATFORM=mysql -e MYSQL_SERVICE_DB_PARAM=characterEncoding=utf8"&"connectTimeout=1000"&"socketTimeout=3000"&"autoReconnect=true"&"useUnicode=true"&"useSSL=false"&"serverTimezone=UTC -p 8848:8848 -d nacos/nacos-server:latest
```

## Consul

```dockerfile
docker run -d --name=dev-consul -p 8500:8500 -e CONSUL_BIND_INTERFACE=eth0 consul:latest
```

```yaml
[2022-03-11 07:51:47,534] INFO [Admin Manager on Broker 1001]: Error processing create topic request CreatableTopic(name='tel', numPartitions=1, replicationFactor=1, assignments=[], configs=[]) (kafka.server.ZkAdminManager)


server:
  port: 9090

spring:
  application:		
    name: yanzhihao-practice
  cloud:
    discovery:
      register: true
    loadbalancer:
      ribbon:
        enabled: false
  datasource:
    type: com.zaxxer.hikari.HikariDataSource
    url: jdbc:mysql://localhost:3306/practice?useUnicode=true&characterEncoding=utf8&allowMultiQueries=true&useLegacyDatetimeCode=false&serverTimezone=Asia/Shanghai
    driver-class-name: com.mysql.cj.jdbc.Driver
    hikari:
      username: root
      password: 123456
      auto-commit: true
      connection-test-query: select 1
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000
      maximum-pool-size: 8
      minimum-idle: 8
      pool-name: HikariCP-financial
  redis:
   host: 127.0.0.1
   port: 6379
   jedis:
     pool:
      max-active: 8
      max-wait: -1ms
      max-idle: 500
      min-idle: 0
   lettuce:
     shutdown-timeout: 0ms

  main:
    allow-bean-definition-overriding: false

  kafka:
    bootstrap-servers: 127.0.0.1:9092
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.apache.kafka.common.serialization.StringSerializer
      bootstrap-servers: 127.0.0.1:9092
      retries: 3
    consumer:
      group-id: default_consumer_group #群组ID
      enable-auto-commit: true
      auto-commit-interval: 1000
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.apache.kafka.common.serialization.StringDeserializer

# mybatis配置
mybatis-plus:
  configuration:
    map-underscore-to-camel-case: true
  global-config:
    db-config:
      # 注意更新策略，默认空值、null值不更新
      update-strategy: not_empty
  mapper-locations: classpath:mapper/*.xml





    
```