# Kafka & Kafka Stream With Java Spring Boot - Hands-on Coding(Udemy)

# Steps to download the Docker 

You can download Docker Desktop for Windows from here - https://docs.docker.com/desktop/windows/install/

Once you able to successfully setup do check and go to setting by right click on docker icon and enable

![image](https://user-images.githubusercontent.com/54174687/141322746-4196bc9c-00de-403d-a0db-657578c7b42b.png)

```
E:\development\kafka>docker --version
Docker version 20.10.10, build b485636

E:\development\kafka>docker-compose --version
Docker Compose version v2.1.1
```
Then try to run simple hello-world example, it should run

```
E:\development\kafka>docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

Attached is the docker-compose.yml with the help of which we'' install the Apache Zookeeper and Apache Kafka

```
version: "3.7"

networks:
  kafka-net:
    name: kafka-net
    driver: bridge

services:
  zookeeper:
    image: zookeeper:3.7.0
    container_name: zookeeper
    restart: always
    networks:
      - kafka-net
    ports:
      - "2181:2181"
    volumes:
      - E:/development/kafka/docker-data/zookeeper:/bitnami/zookeeper

  kafka:
    image: wurstmeister/kafka:2.13-2.7.0
    container_name: kafka
    restart: always
    networks:
      - kafka-net
    ports:
      - "9092:9092"
    volumes:
       - E:/development/kafka/docker-data/kafka:/bitnami/kafka
    environment:
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: DOCKER_INTERNAL:PLAINTEXT,DOCKER_EXTERNAL:PLAINTEXT
      KAFKA_LISTENERS: DOCKER_INTERNAL://:29092,DOCKER_EXTERNAL://:9092
      KAFKA_ADVERTISED_LISTENERS: DOCKER_INTERNAL://kafka:29092,DOCKER_EXTERNAL://${DOCKER_HOST_IP:-127.0.0.1}:9092
      KAFKA_INTER_BROKER_LISTENER_NAME: DOCKER_INTERNAL
      KAFKA_ZOOKEEPER_CONNECT: "zookeeper:2181"
      KAFKA_BROKER_ID: 1
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
    depends_on:
      - zookeeper
```

Make sure to create directory `E:/development/kafka/docker-data/zookeeper` and `E:\development\kafka\docker-data\kafka`, you can keep any path.

```
E:\development\kafka>docker-compose up -d
[+] Running 15/15
 - kafka Pulled                      74.4s
   - 540db60ca938 Pull complete       3.9s
   - 789c480dd801 Pull complete      37.0s
   - 0705a4c47ffe Pull complete      38.6s
   - 661f40345821 Pull complete      64.5s
   - a84fa3c6a2b3 Pull complete      66.2s
 - zookeeper Pulled                  79.8s
   - 7d63c13d9b9b Pull complete      34.0s
   - 225be9814eda Pull complete      36.6s
   - c78f8a9ed884 Pull complete      37.8s
   - 1960c0a25d8b Pull complete      61.7s
   - aa5717ed2807 Pull complete      63.6s
   - c3140920d0d7 Pull complete      65.9s
   - a1361c700299 Pull complete      68.1s
   - 1c80e212099e Pull complete      69.4s
[+] Running 3/3
 - Network kafka-net    Created       0.5s
 - Container zookeeper  Started      20.3s
 - Container kafka      Started
```

# Check Process

```
E:\development\kafka>docker-compose ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
kafka               "start-kafka.sh"         kafka               running             0.0.0.0:9092->9092/tcp
zookeeper           "/docker-entrypoint.…"   zookeeper           running             0.0.0.0:2181->2181/tcp
```

# To Stop 

```
E:\development\kafka>docker-compose stop
```

# Kafka Basic Commands (Docker)

Open command prompt / terminal- Execute `docker exec -it kafka bash`

Execute the script. Don't forget to add .sh extension (e.g. `kafka-topics.sh`)
To exit docker terminal, type exit and press Enter
For example, type the following on command prompt :

1. `C:\> docker exec -it kafka bash`
2. `#> kafka-topics.sh`


# create topic t_hello
`kafka-topics.sh --bootstrap-server localhost:9092 --create --topic t_hello --partitions 1 --replication-factor 1`

# list topic
``kafka-topics.sh --bootstrap-server localhost:9092 --list``

# describe topic
`kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic t_hello`

# create topic t_test
`kafka-topics.sh --bootstrap-server localhost:9092 --create --topic t_test --partitions 1 --replication-factor 1`

# delete topic t_test
`kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic t_test`
