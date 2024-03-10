# DailyGrind

## Use this `docker-compose` file to start all services

```yaml
version: '3'
services:
  zookeeper:
    container_name: zookeeper
    image: 'confluentinc/cp-zookeeper:latest'
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - '2181:2181'
    networks:
      - backend

  kafka:
    container_name: kafka
    image: 'confluentinc/cp-kafka:latest'
    depends_on: 
      - zookeeper
    ports: 
      - '9092:9092'
      - '29092:29092'
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: 'zookeeper:2181'
      KAFKA_ADVERTISED_LISTENERS: 'PLAINTEXT://kafka:9092,PLAINTEXT_HOST://localhost:29092'
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: 'PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT'
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
    networks:
      - backend

  frontend:
    container_name: frontend
    build:
      context: ./Frontend
      dockerfile: Dockerfile
    ports:
      - '3000:3000'
    networks:
      - backend

  api-gateway:
    container_name: api-gateway
    build:
      context: ./API-Gateway
      dockerfile: ./API-Gateway/Dockerfile
    ports:
      - '8080:8080'
    depends_on:
      - kafka
    networks:
      - backend

  post-service:
    container_name: post-service
    build:
      context: ./Post-Service
      dockerfile: ./Post-Service/Dockerfile
    ports:
      - '8081:8081'
    depends_on:
      - kafka
    networks:
      - backend

networks:
  backend:
```

To add necessary env variables for kubernetes:

```
kubectl create secret generic vite-gateway-baseurl --from-literal=VITE_GATEWAY_BASEURL='http://127.0.0.1:8080'
```
