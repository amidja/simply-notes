# Kafka Connector - Getting Started

Kafka Connect is a tool for scalably and reliably streaming data between Apache Kafka and other systems. Below are the steps to get started with Kafka Connect:




## Prerequisites
1. Docker
2. Docker-compose
3. Kafka Connect distribution or Docker image.

## Steps to Set Up Kafka Connect

https://www.mongodb.com/docs/kafka-connector/current/quick-start/
or 
https://kafka.apache.org/quickstart

### 1. Download Tutorial Sandbox

```bash
git clone https://github.com/mongodb-university/kafka-edu.git
cd kafka-edu/docs-examples/mongodb-kafka-base/
```

### 2. Start Sandbox

```bash
sudo apt-get install python3-distutils-extra
docker-compose -p mongo-kafka up -d --force-recreate
```

## References

- [Kafka MongoDB Connector](https://github.com/mongodb/mongo-kafka)
