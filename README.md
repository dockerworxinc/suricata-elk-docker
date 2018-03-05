# suricata-elk-docker

## Running the below script

ip link set eth0 multicast off
ip link set eth0 promisc on
ip link set eth0 up 

## Pull the required images

```
docker pull ubuntu
docker pull docker.elastic.co/elasticsearch/elasticsearch:6.1.1
docker pull docker.elastic.co/kibana/kibana:6.1.1
docker pull docker.elastic.co/logstash/logstash:6.1.1
```

## Running Elasticsearch

```
docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" --hostname=elastic --name=elastic --network=host -t --mount source=elastic,destination=/usr/share/elasticsearch/data docker.elastic.co/elasticsearch/elasticsearch:6.1.1
```

## Running Kibana

```
docker run -e ELASTICSEARCH_URL="http://localhost:9200" --hostname=kibana --name=kibana --network=host -p 5601:5601 -t docker.elastic.co/kibana/kibana:6.1.1
```

## Running Logstash

```
git clone https://github.com/gradiuscypher/grIDS
cd grIDS
docker build -t logstash .
docker run --hostname=logstash --name=logstash --network="host" -e "xpack.monitoring.elasticsearch.url=http://localhost:9200" -t logstash
```

## Running Suricata

```
cd ..
cd suricata/
docker build -t suricata .
docker run --network=host --hostname=suricata --name=suricata -it suricata

```
