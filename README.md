# hello-world
Git Tutorial

Readme is modified. 


설정변경시 Build/배포, Restart등이 발생하지 않도록 설정을 외부화한다.
개발,테스트,운영 등의 여러 프로파일과 다수의 마이크로서비스의 환경설정 속성을 중앙집중적으로 관리 할 필요가 있다.
특정 마이크로서비스의 속성이 변경된 경우 다수의 인스턴스들 사이에 변경내용이 자동으로 전파 되어야 한다.
Spring Cloud Config, Spring Cloud Client – 설정의 외부화, 집중화 
Spring Cloud Bus – 설정변경의 전파
그외 Zookeeper, Consul등이 있다.


Git Repository의 변경을 감지하여 설정정보를 갱신한다.
개별 Service와 설정파일의 매칭은 ServiceID기준이다.
개발관련
하나의 마이크로서비스로 구현된다.
Cloud Config Server component를 사용하여 구현한다.
Pom.xml
<dependency>< groupId>org.springframework.cloud</groupId>< artifactId>spring-cloud-config-server</artifactId>< /dependency>
Main Class에 
@EnableConfigServer
설정 Repository를 지정한다. 
spring.cloud.config.server.git=uri: https://github.com/myorg/{application} 
  
  
Spring Cloud Component를 사용한다.
Config Client
서비스 기동시 와 설정변경 refresh 가 발생 하였을때 Config Server에서 자신의 설정정보를 받아와 local에 caching한다.
이후로는 내부적으로 caching된 설정정보를 사용한다.
Actucator
변경된 설정정보를 Config Server에서 읽어 오도록 하기 위해 해당 서비스의 refres를 호출 하게 되느데 그 종단점(REST API)를 자동으로 제공 해 준다
설정파일변경
Application.properties -> bootstrap.properties로 rename
자신의 Service ID 지정
Spring.application.name=search-service
접속 할 Config Server지정
Spring.cloud.config.url=http://configServerIP:port
Config Server에 등록 할 설정파일작성
파일이름은 serviced.properties : search-service.properties
설정을 사용 할 Class에 다음 Annotation : refresh 시 설정을 변경 하기 위한 annotation
@RefreshScope 


각 Instance간에 설정 변경이 자동으로 전파되게 하기 위한 Component이다.

다수의 Instance로의 ScaleOut 을 전제로 하는 MSA에서 설정변경시 모든 Instance에 /refresh를 호출 해야 하는 것은 부담스럽다.

한 Microservice의 여러개의 Instance에 적용되는 개념이며 이들 Instance들을 하나의 Message Broker에 연결 하여 구현한다



  
  
