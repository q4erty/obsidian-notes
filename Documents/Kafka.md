version: '3'
services:
  zookeeper:
    image: wurstmeister/zookeeper:3.4.6
    environment:
      ZOOKEEPER_SERVER_ID: 1
      ZOOKEEPER_TICK_TIME: 2000
      ZOOKEEPER_SYNC_LIMIT: 2
    ports:
      - "2181:2181"
    networks:
      - kafka-net

  kafka:
    image: confluentinc/cp-kafka:latest
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: "zookeeper:2181"
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_LISTENERS: PLAINTEXT://0.0.0.0:29092,PLAINTEXT_HOST://0.0.0.0:9093
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:29092,PLAINTEXT_HOST://34.118.71.122:9093
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_AUTO_CREATE_TOPICS_ENABLE: "true"
    ports:
      - "29092:29092"  # Внутренний порт для взаимодействия внутри сети Docker
      - "9093:9093"    # Внешний порт для удаленных клиентов
    depends_on:
      - zookeeper
    networks:
      - kafka-net

networks:
  kafka-net:
    driver: bridge



