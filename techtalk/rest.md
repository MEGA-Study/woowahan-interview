# REST

## 5줄 정리

1. REST라는 아키텍처를 구현하는 API
2. 리소스 중심으로 디자인 된다. 리소스를 이름으로 구분하여 해당 자원의 상태를 주고 받는 것. 리소스는 클라이언트가 엑세스 할 수 있는 모든 오브젝트, 데이터, 서비스
3. 각 리소스를 고유하게 식별하는 ID가 있다.
4. 각 리소스에 대한 행동은 HTTP Method로 정의된다. GET /article/1 이런식
5. HTTP 프로토콜을 활용해서 웹의 장점을 살릴 수 있다.

HTTP의 특징을 거의 그대로 옮겨놓은 것이다. 그래서 우리의 API들도 거의 지키고 있지만, 잘 지키지 못하는 것들이 두가지 있는데 그것은 self-descriptive, HATEOAS 이다.

- Self-Descriptive: 메세지의 내용만으로 메세지가 뭘 하는지 알아야 한다.

```html
GET / HTTP /1.1
Host: smjeon.dev
```

위는 Self-Descriptive 하다. '/'로 가서 GET 해와라. 그 자원을 가지고있는 host는 smjeon.dev이다.

```json
HTTP/1.1 200 OK
Content-Type: application/json-path+json  

[ {"op": "remove", "path": "a/b/c/" }
```

이렇게 Content-Type에서 아래 body에 담긴 내용이 어떤 내용인지 알 수 있기 때문에 Self-Descriptive 하다.(json-path+json 명세를 보면 알 수 있다.)

- HATEOAS: 애플리케이션의 상태는 link를 통해서 전이돼야 한다.

```json
HTTP/1.1 200 OK
Content-Type: application/json
Link: </article/1>; rel="previous",
    </article/3>; rel="next";

{
    "title": "The second article",
    "contents": "blah blah.."
}
```

위와 같이 Link 태그로 HATEOAS 하게 만들 수 있다. 여러 가지 방법이 있는데, 잘 이해도 안되고 왜 해야하는지 납득도 잘 안가기 때문에 더이상 알아보지 않는다.

## 상세 설명

REST는 REpresentational State Transfer의 약자이다. 분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍처의 한 형식이다.

엄격한 의미의 REST는 네트워크 아키텍처 원리의 모음이다. `네트워크 아키텍처 원리`란 자원을 정의하고 자원에 대한 주소를 지정하는 방법 전반을 일컫는다.

간단한 의미로는, 웹 상의 자료를 HTTP위에서 SOAP([Simple Object Access Protocol](https://ko.wikipedia.org/wiki/SOAP))이나 쿠키를 통한 세션 트랙킹 같은 별도의 전송 계층 없이 전송하기 위한 아주 간단한 인터페이스를 말한다.

무슨 뜻인가 생각해보면..

- 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것.
- HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 그대로 활용할 수 있다.
- HTTP URI를 통해 자원을 명시하고 HTTP Method를 통해 해당 자원의 행동을 나타낸다. 모든 자원에 고유한 ID를 부여한다.

그래서 RESTful API는 REST의 특징을 가진 API라고 할 수 있다. REST한 구조에 맞는 API 인 듯 하다.

## REST 구성 요소

1. 자원(Resource): URI
   1. 모든 자원에 고유한 ID가 존재.
   2. /groups/:group_id 형식
2. 행위(Verb): HTTP Method
   1. GET, POST, PUT, DELETE
3. 표현(Representation of Resource)
   1. Client가 자원의 상태에 대한 조작 요청을 하면 Server는 적절한 응답(Representation)을 보낸다.
   2. 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타낸다.

## REST 특징

1. Server-Client
   1. Server: 자원이 있는쪽, API를 제공, 비즈니스 로직 처리
   2. Client: 자원 요청하는 쪽, 사용자 인증, 세션, 로그인 정보를 직접 관리
2. Stateless
   1. HTTP가 기본적으로 stateless protocol이다.
   2. Client의 context를 서버에 저장하지 않는다.
   3. 각각의 요청을 아무런 연관관계 없이 별개의 것으로 처리한다.
3. Cacheable
   1. HTTP의 캐싱 기능을 적용할 수 있다.
      1. 불필요한 데이터 전송을 줄인다.
      2. origin server에 대한 request를 줄여준다.
      3. 응답 시간이 빨라진다.
4. Layered System
   1. REST Server는 다중 계층으로 구성될 수 있다.
   2. Proxy, Gateway와 같은 네트워크 기반의 미들웨어를 사용할 수 있다.
5. Code-On-Demand(optional)
   1. Server로부터 스크립트를 받아서 Client에서 실행한다.
6. Uniform Interface(인터페이스 일관성)
   1. URI로 지정한 Resource에 대한 조작을 일관된 인터페이스로 수행한다.
   2. 특정 언어나 기술에 종속되지 않고 HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용가능하다.
