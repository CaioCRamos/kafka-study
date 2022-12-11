# Kafka Study

## Reference material:
- https://developer.confluent.io/tutorials/#learn-the-basics

## Setup Kafka Broker on Docker

```yml
version: '3'
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:7.3.0
    container_name: zookeeper
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000

  broker:
    image: confluentinc/cp-kafka:7.3.0
    container_name: broker
    ports:
      - "9092:9092"
    depends_on:
      - zookeeper
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: 'zookeeper:2181'
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_INTERNAL:PLAINTEXT
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092,PLAINTEXT_INTERNAL://broker:29092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_TRANSACTION_STATE_LOG_MIN_ISR: 1
      KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR: 1
```

### Stark Kafka Broker
```bash
docker-compose up -d
```

### Enter to Kafka Broker's bash
```bash
docker exec -it broker bash
```

### Create topic
```bash
kafka-topics \
    --bootstrap-server broker:9092 \
    --create \
    --topic first-topic
```

### List topics
```bash
kafka-topics \
    --bootstrap-server broker:9092 \
    --list
```

### Describe topics
```bash
kafka-topics \
    --bootstrap-server broker:9092 \
    --describe \
    --topic first-topic
```

### Write messages to the topic
```bash
kafka-console-producer \
    --bootstrap-server broker:9092 \
    --topic first-topic
```

### Read messages from the topic
```bash
kafka-console-consumer \
    --bootstrap-server broker:9092 \
    --topic first-topic \
    --from-beginning # read the entire topic
```

### Write messages to the topic with full key-value pairs
```bash
kafka-console-producer \
    --bootstrap-server broker:9092 \
    --property parse.key=true \
    --property key.separator=":" \
    --topic first-topic 
```

### Read messages from the topic with full key-value pairs
```bash
kafka-console-consumer \
  --bootstrap-server broker:9092 \
  --from-beginning \
  --property print.key=true \
  --property key.separator="-" \
  --topic first-topic 
```