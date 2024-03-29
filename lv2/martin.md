# Lv 2 Review

## 학습 목표

- Spring 프레임워크 기반으로 웹 애플리케이션을 개발하는 경험을 한다.
- TDD, ATDD 기반으로 웹 애플리케이션을 개발하고 리팩토링하는 경험을 한다.
- 구현한 프로그램을 서버에 배포하는 경험을 한다.
- 팀 프로젝트를 통해 팀원들간의 소통, 협업, 회고 경험을 한다.

## 뭘 배웠을까, 받았던 피드백

- 여러 쓰레드가 공유할 수 있는 공유 자원으로 보이네요 :) 아래 블로그 내용 혹은 이펙티브 자바 item 66 변경 가능 공유 데이터에 대한 접근은 동기화하라를 공부해보고 적용해보면 어떨까요? (AtomicLong)
- getter를 통한 비교보다는 객체에 메시지를 전달하는 방법으로 수정해보면 어떨까요?
  - article.getId() 대신 article.isSameId(long id) 사용
- 객체를 조회할 때 Optional에서 바로 예외를 던지기보다는, Optional을 리턴 타입으로 제공해서 유연하게 사용하도록 해보면 어떨까요? 사용하는 도메인 레이어 측에서 orElseThrow로 예외를 던질지, 혹은 null에 대한 추가적인 처리를 할 지 선택할 수 있겠네요. :)
- 세터를 사용하지 않고 입력받기 위한 DTO와 도메인 모델을 분리하고 생성자를 통해 새로운 키를 넣어주며 생성을 해보면 어떨까요?
- 도메인 모델에 대해서도 테스트를 작성하는 습관을 들이면 좋을 것 같아요 :)
- 참조를 넘겨 수정이 일어나서 getId를 사용하는것보다는, id를 가지고있는 Article 엔티티를 리턴하도록 해보면 어떨까요? 자세한 사항은 JPA 영속성 컨텍스트를 공부하시게 되면 알 수 있을 것 같네요. :)
- 전체적인 테스트 작성 👍 다만 테스트코드에 전체적으로 체이닝이 길어 확인이 어려운데 추상화하여 분리해보면 어떨까요?
- DI시에는 field inejction보다는 constructor injection이 권장됩니다. 아래 블로그 참고해보시면 좋을 것 같네요. :)
- 예외에 대한 처리를 위해 ControllerAdvice를 찾아보면 좋을것 같아요!
- @column 을 이용해 unique한 값인지, null을 허용하는지 등에 대해 체크할 수 있어요. 실제로 db에 저장시에도 유효한지 확인하는 것은 중요해요! 관련해서 찾아보고 적용하면 좋을 것 같아요!
- 회원가입시, 이름 비밀번호등이 형식에 맞지 않는 경우, 어떤 형식으로 작성해야하는지 문구에 같이 표시가 되어야 할 것 같아요!
- AllArgsConstructor 사용은 지양, 필요한 Field로 Constructor 를 직접 만들면 어떨까요~? 같은 타입의 필드 선언 순서를 나중에 바꿀 경우 문제가 발생 소지가 있습니다. 개인적으로 생성자를 명시적으로 놓고 사용한다면 DI이후 필드값에 대한 추가적인 설정 또한 용이합니다.
- CQRS (Command Query Responsibility Segregation) - 아직 잘 모르겠음
- Ajax는 클라이언트 사이드의 개발 기술이고, Rest는 http request를 핸들링하고 보내기 위한 구조적인 것
- javascript multiline string으로 검색해보고 적용해볼까요?

## JPA 영속성 컨텍스트

엔티티를 영구 저장하는 환경

1. 비영속 상태
   - 엔티티 객체를 생성한 상태. 영속성 컨텍스트랑 아무런 관련이 없다.
2. 영속 상태
   - 엔티티 매니저를 통해 엔티티를 영속성 컨텍스트에 저장했다. 이제 영속성 컨텍스트에 의해 관리 된다.
3. 준영속 상태
   - 영속 상태의 엔티티를 영속성 컨텍스트가 관리하지 않으면 준영속 상태가 된다. `em.detach(), em.clear()`
4. 삭제
   - 엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제한다. `em.remove(member)`

영속성 컨텍스트가 엔티티를 관리하면

1. 1차 캐시
2. 동일성 보장
3. 트랜잭션을 지원하는 쓰기 지연
4. 변경 감지
5. 지연 로딩

### 변경 감지(영속 상태의 엔티티에만 적용된다)

1. 스냅샷과 플러시 시점의 엔티티를 비교해서 변경된 엔티티를 찾는다.
2. 변경된 엔티티가 있으면 수정 쿼리를 생성해서 쓰기 지연 SQL 저장소에 보낸다.
3. 쓰기 지연 저장소의 SQL을 데이터베이스에 보낸다.
4. 데이터베이스 트랜잭션을 커밋한다.

### 모든 필드를 업데이트하는 것의 장점

- 수정 쿼리가 항상 같다. 따라서 애플리케이션 로딩 시점에 수정 쿼리를 미리 생성해두고 재사용할 수 있다.
- 데이터베이스에 동일한 쿼리를 보내면 데이터베이스는 이전에 한 번 파싱된 쿼리를 재사용할 수 있다.

원하는 필드만 업데이트 하려면 DynamicUpdate를 사용하자.

### flush()

영속성 컨텍스트의 변경 내용을 데이터베이스에 반영한다. 변경 내용을 데이터에비스에 동기화 하는 것

1. em.flush() 직접호출
2. 트랜잭션 커밋 시 플러시가 자동 호출된다.
3. JPQL 쿼리 실행 시 플러시가 자동 호출된다.

JPA 공부하자..

## 롬복 사용시 주의사항

1. AllArgsConstructor, RequiredArgsConstructor 사용 금지
   - 필드의 선언 순서가 바뀌면 경우에 문제 발생 가능
   - `private int myMoney; private int yourMoney;`에서 필드 순서를 바꾸면 생성자의 순서도 바뀌는데, 기존 코드의 수정은 필요없기 때문에, 애초에 개발자(생성자를 호출한 사람)가 의도한 yourMoney는 myMoney로 들어가고 myMoney는 yourMoney로 들어갈 것이다.
2. EqualsAndHashCode(of="필드")와 같은 방식으로만 사용
3. @Data 사용금지 (@EqualsAndHashCode, @RequiredArgsConstructor 포함되어있음)
4. @Value 사용금지, 위와 동일 이유
5. @Builder는 직접 만든 생서자 혹은 static 객체 생성 메서드에만

## DI - Constructor Injection 사용 이유

### Field Injection

1. 단일 책임의 원칙 위반
   - 의존성을 주입하기가 쉽다. @Autowired 선언 아래 3개든 10개든 막 추가할 수 있으니 말이다. 여기서 Constructor Injection을 사용하면 다른 Injection 타입에 비해 위기감 같은 걸 느끼게 해준다. Constructor의 파라미터가 많아짐과 동시에 하나의 클래스가 많은 책임을 떠안는다는 걸 알게된다. 이때 이러한 징조들이 리팩토링을 해야한다는 신호가 될 수 있다.
2. 의존성이 숨는다.
   - DI(Dependency Injection) 컨테이너를 사용한다는 것은 클래스가 자신의 의존성만 책임진다는게 아니다. 제공된 의존성 또한 책임진다. 그래서 클래스가 어떤 의존성을 책임지지 않을 때, 메서드나 생성자를 통해(Setter나 Contructor) 확실히 커뮤니케이션이 되어야한다. 하지만 Field Injection은 숨은 의존성만 제공해준다.
3. DI 컨테이너의 결합성과 테스트 용이성??? [https://www.vojtechruzicka.com/field-dependency-injection-considered-harmful/](https://www.vojtechruzicka.com/field-dependency-injection-considered-harmful/)
   - DI 프레임워크의 핵심 아이디어는 관리되는 클래스가 DI 컨테이너에 의존성이 없어야 한다. 즉, 필요한 의존성을 전달하면 독립적으로 인스턴스화 할 수 있는 단순 POJO여야한다. DI 컨테이너 없이도 유닛테스트에서 인스턴스화 시킬 수 있고, 각각 나누어서 테스트도 할 수 있다. 컨테이너의 결합성이 없다면 관리하거나 관리하지 않는 클래스를 사용할 수 있고, 심지어 다른 DI 컨테이너로 전환할 수 있다.
   - DI Container외부에서는 그 클래스를 재사용할 수 없다. 필드 주입은 해당 인스턴스를 만드는데 모든 필요한 의존성을 다 주입해줄 방법이 없다.?
하지만, Field Injection을 사용하면 필요한 의존성을 가진 클래스를 곧바로 인스턴스화 시킬 수 없다.
4. 불변성(Immutability)
   - Constructor Injection과 다르게 Field Injection은 final을 선언할 수 없다. 그래서 객체가 변할 수 있다.
5. 순환 의존성
   - Constructor Injection에서 순환 의존성을 가질 경우 BeanCurrentlyCreationExeption을 발생시킴으로써 순환 의존성을 알 수 있다.
   - 순환 의존성이란? First Class가 Second Class를 참조하는데 Second Class가 다시 First Class를 참조할 경우 혹은 First Class가 Second Class를 참조하고, Second Class가 Third Class를 참조하고 Third Class가 First Class를 참조하는 경우 이를 순환 의존성이라고 부른다.

## Logger

TRACE < DEBUG < INFO < WARN < ERROR

```properties
logging.level.root=TRACE
logging.level.techcourse.myblog.web=DEBUG
```

- ERROR – something terribly wrong had happened, that must be investigated immediately.
- WARN – the process might be continued, but take extra caution.
- INFO – Important business process has finished.
- DEBUG – Developers stuff.
- TRACE – Very detailed information, intended only for development.

1. Don't forget, logging levels are there for you
2. 뭘 로깅하는지 알아야한다. 무작정 로그찍고 그러지 말자
3. 성능, 기능에 영향을 미치면 안된다. (NPE)
4. 명확하고 표현을 잘 해야한다.
5. Argument, Return value를 logger로 찍어두면 디버거를 못쓰는 경우에 유용하게 쓸 수 있다
   - 중복코드가 계속 발생할 수 있는데.. AOP로 해결 할 수 있다. (AOP를 공부하자...)
6. 외부 시스템과 연동할 경우 logging을 더 잘하자

## Open Session In View

영속성 컨텍스트를 뷰 렌더링이 끝나는 시점까지 개방한 상태로 유지하는 것

### Lazy Loading

- 즉시로딩(EAGER)
  - @ManyToOne
  - @OneToOne
- 지연로딩
  - @OneToMany
  - @ManyToMany

연관된 데이터를 지연로딩할 때 프록시 객체가 사용된다. Lazy Loading 전략에 따라 연관 관계를 맺고 있는 객체와 컬렉션에는 실제 Entity 대신 프록시 객체가 생성된다.

```java
@Entity
public class User {
    @Id
    private Long id;

    @Column(nullable = false)
    private String name;

    @OneToMany
    private List<Article> articles;
}
```

여기서 `List<Article>`은 프록시 객체로 채워진다. 여기서 이제 articles를 사용하려고 하는 순간 Fetch되어 쿼리가 실행된다. List를 가져오는 시점이 아닌, articles.iterator()를 통해 프록시 객체의 데이터에 접근할 때 프록시 객체의 초기화가 이루어진다.

```java
//Controller
@GetMapping("")
public String home(Model model) {
    model.addAttribute("teams", teamService.findAll());
    return "home";
}

//Service
@Transactional
public List<Team> findAll(){
    return teamRepository.findAll();
}
```

findAll() 메서드가 종료될 때 Transaction이 종료되고, Transaction 종료로 JDBC Connection이 disconnect, Hibernate Session이 종료되고, 영속 객체는 Detached 상태가 된다.

이 문제를 해결하기 위해서 다른 방법도 있지만 이해하려고 하지 않았다. Open Session In View 패턴만 살펴보면..

영속성 컨텍스트를 오픈된 채로 뷰 렌더링 시점까지 유지하는 것.

1. 전통적인 Open Session In View
   1. 서블릿 필터 시작 시에 Hibernate Session을 열고 트랜잭션을 시작한다.
   2. 뷰 렌더링이 모두 완료된 후에 트랜잭션을 커밋 or 롤백
   3. 뷰까지 트랜잭션이 확장된다.. - 모호한 트랜잭션
   4. JDBC 커넥션 보유시간 증가..
2. Spring의 Open Session In View
   1. OpenSessionInViewFilter, OpenSessionInViewInterceptor
      1. 뷰에서 지연로딩을 가능하게 하면서 서비스 레이어에 트랜잭션 경계를 선언할 수 있다.
      2. OpenSessionInViewFilter는 필터 내에서 Session을 오픈하지만 트랜잭션을 시작하지 않는다.

트랜잭션이 종료된 후에도 Controller의 Session이 close 되지 않았기 때문에 영속 객체는 persistence 상태를 유지할 수 있고, persistence 상태이기 때문에 lazy loading을 수행 할 수 있다.

무슨 소리인지 잘 모르겠다.. 결국엔 스프링 부트에서는 다 된다. 스프링부트에서는 spring.jpa.open-in-view=true 이다.

한마디로 뷰 렌더링 시점에 객체를 사용해야하는데 Lazy Loading 때문에 프록시 객체였던 친구가 detached 되었기 때문에 찾을 수 없는 객체가 되어서 실제 렌더링에 사용할 수 없게 되었던 문제를, 영속성 컨텍스트를 뷰 렌더링 할 때까지 열어두는 방식으로 고쳤다. 라는 것 같다.

- 참고자료: [Open Session In View](https://kingbbode.tistory.com/27)

## Spring MVC 프레임워크 동작 방식

1. Dispatcher Servlet은 Request를 받는다.
2. HandlerMapping 객체에게 Controller 검색을 요청
3. HandlerMapping은 클라이언트의 요청 경로를 이용해서 이를 처리하는 Controller를 찾아서 DispatcherServlet에 리턴
4. HandlerAdapter에 클라이언트의 요청 처리를 위임한다.
5. HandlerAdapter는 컨트롤러의 메서드를 호출해서 요청을 처리한다.
6. 처리한 결과를 ModelAndView에 담아서 DispatcherServlet에 리턴한다.
7. DispatcherServlet은 ModelAndView를 가지고 ViewResolver를 이용해서 결과를 보여줄 뷰를 찾는다.
8. ViewResolver는 ModelAndView 객체에 담긴 ViewName으로 View 객체를 찾거나 생성해서 리턴한다.
   1. ViewResolver 는 매번 새로운 View 객체를 생성해서 DispatcherServlet에 리턴한다.
9. DispatcherServlet 은 ViewResolver 가 리턴한 View 객체에게 응답 결과 생성을 요청한다.
10. View 객체는 응답 결과를 생성한다. (render)
    1. JSP를 사용하는 경우, View 객체는 JSP를 실행함으로서 브라우저에게 전송할 응답 결과를 생성한다. ModelAndView 의 Model 객체에 담겨 있는 데이터가 응답 결과에 필요하면 Model 에서 데이터를 꺼내 JSP 에서 사용할 수 있다.

웹 브라우저의 요청이 들어오면 DispatcherServlet은 아래와 같이 행동한다.

1. RequestMappingHandlerMapping 을 사용해서 요청을 처리할 핸들러를 검색한다.
   1. 존재하면 해당 컨트롤러를 이용해서 요청을 처리한다.
   2. 존재하지 않으면 SimpleUrlHandlerMapping 을 사용해서 요청을 처리할 핸들러를 검색한다.
      1. SimpleUrlHandlerMapping 은 모든 경로("/**")에 대해 DefaultServletHttpRequestHandler를 리턴한다.
      2. DispatcherServlet 은 DefaultServletHttpRequestHandler 에 처리를 요청한다.
      3. DefaultServletHttpRequestHandler 는 디폴트 서블릿에 처리를 위임한다.

이 부분은 잘 모르겠어요

## 팀 프로젝트에서 배운 것

- CI/CD
  - 수시로 빌드, 배포, 테스트
  - git flow와 같은 브랜칭 전략을 활용하자.
  - 깃헙으로 예시를 들면 dev 브랜치에서 master 브랜치로 pr을 날리고, 코드리뷰 이후 merge가 되었다 -> 자동화된 빌드 수행 -> 자동화 테스트 수행 -> 빌드 결과 확인 후 깨진다면 우선순위를 높여서 수정 -> 이후 운영 서버에 배포가 필요하다면 CI 서버에서 자동화 한다.
- 깃 브랜칭 전략
  - dev branch와 각 기능별 브랜치를 나누고 dev branch로 PR 날리고 머지되면 배포(자동화)
- AWS EC2, S3
  - 어떤 개념인지? 어떤 용도로 썼는지, 사용법 정도 파악 했다.
- Reverse proxy
  - 특정 서버에 대한 요청이 반드시 경유하도록 설치된 프록시 서버
  - 특정 서버들만을 목적지로 한다.
  - 특정 서버의 부하를 줄이거나, 접속을 제한하여 보안 강화
- Load balancing
  - 집중되는 부하를 여러 곳으로 나누어 처리
  - 병렬 운영되는 장비들에 부하를 균등 배분
  - 둘 이상의 장치들에게 작업을 배분하는 일
- Docker, Jenkins, Nginx 등

기술 측면 제외하고 받았던 피드백은

1. 질문을 했으면 좋겠다.
2. 혼자 하려고 하기보다는 같이 고민해볼 수 있도록 주변에 알렸으면 좋겠다.
3. 주변 사람의 도움을 받았으면 좋겠다.
4. 흥미가 떨어지는 부분도 관심을 가져줬으면 좋겠다.
5. 의견을 명확하게 표현 했으면 좋겠다.
