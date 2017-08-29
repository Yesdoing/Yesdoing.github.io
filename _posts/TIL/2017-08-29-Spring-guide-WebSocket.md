---
layout: post
title: "WebSocket"
categories:
  - TIL
tags:
  - SpringBoot
  - WebSocket
  - JavaScript
last_modified_at: 2012-01-30T12:25:10-05:00
---

#스프링부트 가이드 번역
## WebSocket을 사용하여 대화 형 웹 응용 프로그램 만들기
  이 가이드는 브라우저와 서버 사이에서 메시지를 앞뒤로 보내는 "hello world" 응용 프로그램을 만드는 과정을 소개합니다. WebSocket은 TCP 위에있는 매우 얇고 가벼운 레이어입니다. 웹소켓은 "하위 프로토콜"을 사용하여 메시지를 포함시키는 것이 매우 적합합니다. 이 가이드에서는 Spring에서 STOMP 메시징을 사용하여 대화 형 웹 애플리케이션을 만들어 봅니다.

## 무엇을 만듭니까?
  전달되는 사용자의 이름 메시지를 받을 서버를 만듭니다. 서버는 이에 응답하여 클라이언트가 구독하는 큐로 인사말을 푸시합니다.

## 무엇이 필요합니까?
  * 약 15분
  * 자신이 쓰는 텍스트 에디터 또는 IDE
  * JDK 1.8버전 이상
  * Gradle 2.3+ 또는 Maven 3.0+

## 이 가이드를 마무리하는 방법
  대부분의 Spring Getting Started 가이드와 마찬가지로, 처음부터 시작하여 각 단계를 완료하거나 이미 익숙한 기본 설정 단계를 건너 뛸 수 있습니다. 어쨌든, 당신은 작업 코드로 끝납니다.

  기본사항을 건너 뛰려면
  * 이 가이드의 소스 저장소를 [다운로드](https://github.com/spring-guides/gs-messaging-stomp-websocket/archive/master.zip)하고 압축을 해제하거나 Git을 사용하여 복제합니다. Git:```
    git clone https://github.com/spring-guides/gs-messaging-stomp-websocket.git```
  * gs-messaging-stomp-websocket/initial로 이동하십시오.
  * 클래스 자원 정의 메뉴로 이동하세요.

  작업이 끝나면 ```gs-messaging-stomp-websocket / complete```의 코드와 결과를 비교할 수 있습니다.

  제가 Maven으로 작업해서 Maven 부분만 번역합니다.

## Maven을 사용해서 프로젝트 빌드하는 법
  먼저 기본 빌드 스크립트를 설정합니다. Spring을 사용하여 응용 프로그램을 빌드 할 때 원하는 빌드 시스템을 사용할 수 있지만 Maven을 사용하여 작업할 때 필요한 의존성이 있습니다. Maven에 익숙하지 않은 경우 [Maven을 사용하여 Java 프로젝트 빌드하는 법](https://spring.io/guides/gs/maven)을 보세요.

###디렉토리구조 만들기
  선택한 프로젝트 디렉토리에서 다음과 같은 하위 디렉토리 구조를 만듭니다. * nix 시스템 예 : ```mkdir -p src / main / java / hello on```

  <pre class="prettyprint">└── src
    └── main
        └── java
            └── hello</pre>

  ```pom.xml```
  ```
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-messaging-stomp-websocket</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.5.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-websocket</artifactId>
        </dependency>

        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>webjars-locator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>sockjs-client</artifactId>
            <version>1.0.2</version>
        </dependency>
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>stomp-websocket</artifactId>
            <version>2.3.3</version>
        </dependency>
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>bootstrap</artifactId>
            <version>3.3.7</version>
        </dependency>
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.1.0</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

  </project>
  ```
### Spring Boot Maven 플러그인은 많은 편리한 기능을 제공합니다 :
* 너의 서비스를 보다 편리하게 전달하고 실행하기 위해 클래스 패스에 있는 모든 jar파일들을 모아 하나의 "über-jar"를 만들어줍니다.

* ```public static void main () ```메서드를 검색하여 실행 가능 클래스로 플래그를 지정합니다.

* 스프링 부트 의존성에 맞게 버전 번호를 설정하는 빌트인 의존성 분석기를 제공합니다. 원하는 모든 버전을 무시할 수 있지만 기본적으로 Boot에서 선택한 버전 세트가 사용됩니다.


### 클래스를 정의해보자.
  이제 프로젝트를 설정하고 시스템을 구축 했으므로 STOMP 메시지 서비스를 만들 수 있습니다.

  서비스 상호 작용에 대해 생각함으로써 프로세스를 시작하십시오.

  이 서비스는 body가 JSON 객체 인 STOMP 메시지에 포함 된 name 메시지를 받습니다. 주어진 name이 "Fred"이면 메시지의 내용은 다음과 같습니다.

  ```
    {
      "name": "Fred"
    }
  ```

  name을 전달하는 메시지를 모델링하기 위해, 당신은 name 프로퍼티와 일치하는 ```getName()```메서드를 plain old Java object(POJO)로 만들어야합니다.

  ```src/main/java/hello/HelloMessage.java```

  ```
  package hello;

  public class HelloMessage {

  private String name;

  public HelloMessage() {
  }

  public HelloMessage(String name) {
      this.name = name;
  }

  public String getName() {
      return name;
  }

  public void setName(String name) {
      this.name = name;
    }
  }
  ```

  메시지를 수신하고 name을 추출하면 서비스는 인사말을 만들고 클라이언트가 구독하는 별도의 큐에 인사말을 게시하여 메시지를 처리합니다. 인사말은 JSON 객체로 구성됩니다. JSON 객체는 다음과 같습니다.
  ```
    {
      "content": "Hello, Fred!"
    }
  ```

  인사말(greeting)을 정의하기 위한 모델링으로, 당신은 ```content```프로퍼티와 ```getContent()```메서드를 가진 또 다른 plain old Java object를 추가합니다.

  ```src/main/java/hello/Greeting.java```

  ```
  package hello;

  public class Greeting {

  private String content;

  public Greeting() {
  }

  public Greeting(String content) {
      this.content = content;
  }

  public String getContent() {
      return content;
    }

  }
  ```

  Spring은 Jackson JSON 라이브러리를 사용하여 ```Greeting``` 타입의 인스턴스를 JSON으로 자동 생성합니다.


  다음으로 hello 메시지를 받고 greeting 메시지를 보낼 컨트롤러를 만듭니다.


### message-handling 컨트롤러 작성
  STOMP 메시징 작업을위한 Spring의 접근 방식에서 STOMP 메시지는 ```@Controller``` 클래스로 라우팅 될 수 있습니다. 예를 들어, ```GreetingController```는 "/ hello"대상에 대한 메시지를 처리하도록 매핑됩니다.

  ```src/main/java/hello/GreetingController.java```

  ```
  package hello;

  import org.springframework.messaging.handler.annotation.MessageMapping;
  import org.springframework.messaging.handler.annotation.SendTo;
  import org.springframework.stereotype.Controller;

  @Controller
  public class GreetingController {


      @MessageMapping("/hello")
      @SendTo("/topic/greetings")
      public Greeting greeting(HelloMessage message) throws Exception {
          Thread.sleep(1000); // simulated delay
          return new Greeting("Hello, " + message.getName() + "!");
      }

  }
  ```
