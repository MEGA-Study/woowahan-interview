## Spring Boot의 장점

1. 의존 추가를 쉽게 해준다.
    - Spring에서 의존성 추가할 때 (Maven)
     ```
        <dependency>
        	<groupId>org.thymeleaf</groupId>
        	<artifactId>thymepleaf-spring4</artifactId>
        	<version>2.1.4.RELEASE</version>
        </dependency>
      ```
    - 모든 dependency를 버전까지 정확하게 추가해줘야한다.

    - Spring Boot에서는
        - 길이가 짧다.
        - 버전관리도 권장 버전으로 자동 설정되기때문에 버전을 명세해줄 필요가 없다.
        - `spring-boot-starter-***`와 같은 의존은 추가 시 관련된 모든 의존이 자동으로 추가된다.

2. 설정 관리를 쉽게 해준다.

- Spring에서는 추가 설정이 필요한 경우, @Configuration 어노테이션과 함께 코드로 설정해줘야 한다.

    → 복잡하면서 번거롭다.

- Spring Boot에서는 이를 보다 직관적으로 편리하게 관리할 수 있다.
    - application.properties
        ```
        # h2 db console에 접근 설정
        spring.h2.console.enabled=true
        spring.h2.console.path=/h2-console
        
        # Datasource 설정
        spring.datasource.url=jdbc:h2:mem:testdb
        spring.datasource.driverClassName=org.h2.Driver
        spring.datasource.username=sa
        spring.datasource.password=1234
        spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
        spring.jpa.show-sql=true
        spring.jpa.properties.hibernate.format_sql=true
        
        logging.level.techcourse.myblog=DEBUG
        ```
    - application.yml
        ```
        spring:
          profiles:
            active: local # 기본 환경 선택
          jpa:
            properties:
              hibernate:
                dialect: org.hibernate.dialect.MySQL5InnoDBDialect
          devtools:
            livereload:
              enabled: true
        mybatis:
          # config-location: classpath:mybatis-config.xml
          type-aliases-package: com.cota.dto
          mapper-locations:
          - mapper/**.xml
          configuration:
            map-underscore-to-camel-case: true
        ```
    그리고 요즘에는 YAML로 설정 파일을 관리하는 것이 더 선호된다.
    application.yml을 보더라도 계층 구조로 보다 직관적이면서 중복도 제거할 수 있어 편리하기 때문이다.

3. 내장 서버를 가지고 있다.

- Spring은 서버(ex. tomcat)를 외부에서 가져와서 설치하고 연결해야하는 작업이 필요하다.
- Spring Boot는 내장 서버를 가지고 있어서 위와 같은 작업이 필요 없다.
    - 서버 가동시간이 절반 가까이 단축된다.
    - 내장 서블릿 컨테이너 덕분에 jar로 간단하게 배포가 가능하다.

        `java -jar $REPOSITORY/$JAR_NAME &`


## 한 줄 요약

**Spring Boot는 단독적으로 상용화 수준의 스프링 기반 어플리케이션을 만드는데 쉽게 해준다.**

Spring Boot makes it easy to create stand-alone,
production-grade Spring based Application that you can "just run".
