# ELK stack

- ELK설치
- http://rea1man.tistory.com/entry/ELK-elasticsearch-logstash-kibana-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%82%AC%EC%9A%A9
- http://www.yongbok.net/blog/real-time-visitor-analysis-with-logstash-elasticsearch-kibana/

#### ElasticSearch  9200
- (1) 다운&설치

```
wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-1.5.2.tar.gz
```

- (2) 기본적인 plugin설치 http://develop.sunshiny.co.kr/1010

```
$> bin/plugin --list
$> bin/plugin --remove head
$> bin/plugin --install mobz/elasticsearch-head   //http://localhost:9200/_plugin/head/
$> bin/plugin --install lukas-vlcek/bigdesk     //http://localhost:9200/_plugin/bigdesk/

$> bin/plugin --install enezes/elasticsearch-kopf  // http://localhost:9200/_plugin/kopf/

# elasticSearch plugin으로 설치
$> bin/plugin install shihpeng/elasticsearch-kibana // http://localhost:9200/_plugin/kibana/#/dashboard
```

- (3) 실행

```
$> bin/elasticsearch & (d는 백그라운드로)
$> curl 'http://localhost:9200/?pretty' 를 입력해서 결과가 제대로 오는지 체크
```

#### LogStash
- (1) 다운 설치

```
wget https://download.elasticsearch.org/logstash/logstash/logstash-1.4.5.tar.gz
```
- (2) 설정 : conf/collect.conf

- (3) 실행
```
$> bin/logstash -f conf/collect-apachelog.conf > logs/logstash.log  2>&1 &  // -f : 설정파일지정

$> bin/logstash -e 'input { stdin { } } output { elasticsearch { host => localhost } }'  // stdin 으로 들어오는 메세지를 Elasticsearch로
$> curl 'http://localhost:9200/_search?pretty'  // Elasticsearch에서 확인
```

#### Kibana (따로 실행 필요없음 Plugin)
- (1) 설치
```
wget https://download.elastic.co/kibana/kibana/kibana-4.1.1-linux-x64.tar.gz
```
- (2) 설정 : config/kibana.yml
```
$> vi config/kibana.yml

# Kibana is served by a back end server. This controls which port to use.
port: 5601
# The Elasticsearch instance to use for all your queries.
elasticsearch_url: "http://localhost:9200"
```

- (3) 실행
```
$> bin/kibana  
http://localhost:5601
```
