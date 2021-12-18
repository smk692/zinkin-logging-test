# 분산 처리 로그 시스템 테스트 코드 (ZIPKIN)

https://zipkin.io/ ZIPKIN 홈페이지

[github.com/openzipkin/zipkin](github.com/openzipkin/zipkin)
오픈 소스를 사용했습니다.

## 설치 및 실행

#### Jar 또는 docker 를 이용한 ZIPKIN 실행 

```zsh
curl -sSL https://zipkin.io/quickstart.sh | bash -s
java -jar zipkin.jar

또는 

# Note: this is mirrored as ghcr.io/openzipkin/zipkin
docker run -d -p 9411:9411 openzipkin/zipkin

http://localhost:9411/zipkin/ 해당 홈페이지가 켜지면 성공 

```

```zsh
# Kafka 또는 메세지 큐도 하나의 로그에 묶여서 쌓이는지 확인해야 됨으로 카프카도 사용합니다.
git clone https://github.com/wurstmeister/kafka-docker.git
cd kafka-docker
vim docker-compose-single-broker.yml

# Host -> local ip 로 변경 후 저장
KAFKA_ADVERTISED_HOST_NAME: 127.0.0.1
KAFKA_CREATE_TOPICS: "backend:1:1"

# 해당 파일 위치에서 실행 
docker-compose -f docker-compose-single-broker.yml up
docker exec -it 컨테이너ID /bin/sh

# docker container 내부에서 > topic 생성 > 실행   
kafka-topics.sh --list --zookeeper zookeeper:2181
kafka-console-consumer.sh --topic backend --bootstrap-server localhost:9092 
```

#### BackendApplication, FrontApplication 실행
```zsh
curl http://localhost:8080/order/request

쌓인 로그는 http://localhost:9411/zipkin/ 에서 확인한다. 
```

### 내가 생각하는 zipkin 장점과 단점
장점
- 비동기식 또는 SQS 진행되는 로그를 연결해서 볼 수 있음
- 로그관리를 따로 할 필요 없음

단점
- zipkin을 띄어야 하는 서버가 필요 
- 정확한 로그내용은 알기 어려움
  
#### 결론 : zipkin 에서는 LOG의 내용을 정확히 알기 어렵기 때문에 Elasticsearch + Kibana 랑 같이 사용 시 용이 할 것 같음


###By Son(손)
