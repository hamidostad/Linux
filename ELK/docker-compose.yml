version: '3.5'

services:

  elasticsearch:
    image: elasticsearch:7.6.1
    container_name: elasticsearch
    environment:
      - node.name=elasticsearch
      - discovery.seed_hosts=elasticsearch
      - cluster.initial_master_nodes=elasticsearch
      - cluster.name=docker-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - ./elastic-kibana_esdata1/_data:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    user: $UID:$GID
    networks:
      - elk

  kibana:
    image: kibana:7.6.1
    container_name: kibana
    environment:
      ELASTICSEARCH_URL: "http://elasticsearch:9200"
    ports:
      - 5601:5601
    depends_on:
      - elasticsearch
    networks:
      - elk

networks:
  elk:
    driver: bridge
