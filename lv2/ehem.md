# 스프링

## 특징

### IoC(Inversion of Control): 제어의 역전

- 스프링 이전에는 개발자가 프로그램의 흐름을 제어하는 주체
- 스프링은 객체를 생성하고 생명주기를 관리하는 것까지 맡으면서 제어권을 갖게 됨
- IoC를 왜 쓰지?
    - 개발자가 비즈니스 로직에 좀 더 집중하게 하기 위해서 ⇒ 관심사의 분리
    - [https://www.wbluke.com/9](https://www.wbluke.com/9)

⇒ DI, AOP가 가능해짐

### Java Bean vs. Spring Bean

**Java Bean**

- 기본생성자가 존재해야한다.
- 모든 멤버변수의 접근제어자는 private이다.
- 멤버변수마다 getter/setter가 존재해야한다. (속성이 boolean일 경우 is를 붙힘)
- 외부에서 멤버변수에 접근하기 위해서는 메소드로만 접근할 수 있다.
- Serializable(직렬화)가 가능해야한다.

**Spring Bean**

- Spring이 관리하는 인스턴스
- 기본적으로 싱글톤의 scope를 가짐

    → Bean으로 지정된 클래스는 Container에서 1개의 인스턴스로만 존재할 수 있음

### POJO(Plain Old Java Object)

- EJB 상속이 일어나는 순간 확장이 불가능한 종속적인 존재가 되어 평범한 자바 객체로 돌아가자
- 특정 환경에 종속되지 않음
- 객체를 사용하는 프레임워크가 없어져도 비즈니스는 정상적으로 작동할 수 있음

    ~~(애너테이션 기반으로 바뀌면서 딱히 그렇지도 않다고 하던데)~~

## 스프링을 처음 써 본 것에 대한 느낌은 어땠니

- 그래서 프레임워크가 뭔데?
    - 프레임워크란... (머리로만 알고 있을 시절)
- 사용자가 직접 객체를 생성해 주지 않아도 스프링이 매핑해서 넣어준다는 것이 제일 신기했음
    - 당시의 내 머리로는 믿을 수가 없었지...
    - 스프링의 빈 주입 과정
        - 
- 객체도 알아서 생성하고 매핑도 알아서 해주고 실행만 시키면 알아서 웹 서버가 동작하는데 그러면 개발자가 하는 건 뭐냐!

## 서블릿 컨테이너 vs. 어플리케이션 컨텍스트(= DI 컨테이너 = IoC Container)

웹 서버 - WAS ⇒ FM적인 방식

톰캣의 성능이 높아져서 굳이 정/동적 파일을 분리해서 처리해주지 않아도 된다라고 8년 전에 이일민씨가 말씀

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a62df805-fc9a-4dfa-bf54-d00561b653c7/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAT73L2G45EP3J53JC%2F20191027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20191027T041912Z&X-Amz-Expires=86400&X-Amz-Security-Token=AgoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJHMEUCICk30A%2F7d2ayFPwoIr%2BRoizd1HqKlAiE2p2Rac%2FC7jp9AiEAsqWrQ8xkKDC9F%2FgXfeViM0T8aKIb6n3aTEh8LsCbp7sq%2FAMIaxAAGgwyNzQ1NjcxNDkzNzAiDAUokfzzqlYcXbIMxCrZA%2BI3NUzoTcgJW%2BFBwLmDJJG%2B7JIdIoZgG4swwRkmiW9ci%2FPYUsaPW5n%2BSSJTkPw4rfKJbXYbhm1ebVKMEc2o0zBpG3GcJvZT%2FNOHlmShPszDT6wFBCobHQ1ZFFaxvgJI15hEJiROrzNLqYrHKgFdKRpIl%2FVIf4yVGU6PQ1J3Bc1ThYhFmdL%2FufnoW3i6tQjUDldKXFn3BulQDZpYhr4b6ECfYyTolZP05oiZRoUzYUjpBdZEVJMFMyZVXKG3PPWyKg1ihDb5fvXru2yq33XoA0F7hGpSVaNoRZdc6a1WMRFzqPDJD1Pt4uDRw764WavmXNorYY8iTpVc3PUJSWHiFDzgKyNWnbR5NyB74sIrGHzgzFcV%2Faqv6%2F2%2FNMyjKP3N4%2FkSteb6o0BZ0YIw%2BtJPh6OM2mtPAahDLQPhiZU29rHUgapAsAhXUJQqJsrpyoTVwuGe0wiLhyRTf89aCqW1udrGqaIZL7I54z9kbqjHWiY1IGUqVcTJJ6hivX5p85u5Twunb3LM0wxTLLygfSiLHe%2FXxGxrDsRotuC6BRtv0iP4zCqu2S9AX7ZtNM4SsDlZiAekcLXTfNig7%2B0pN7NjixcBvVYqS0Hp6fNefoK6q2vr5H%2FYWbMhfsQmMO%2Fu0%2B0FOrQBTZRng8jR6jbUuWOl%2Fp1JoQWuycJoNJoU7hPURojov5hse8PsARqVnMWdJrEZpxx6ceNixjccMd2t9WNZ04HtGUO43rLmEfAugsXVMIRXinLDC0cOF9dEB8kuYa4M5vMmHJ2Bk0tA7c%2BIJKjZSHXgS2VeZQJph94UrM%2BjGKFVhumfrXgRA9tw3qnwzpZsnlaQ9Ls19iWTuhsEa5KyncKgfT2N9XdpLDWQjs0P4hqZ4G8eKuSE&X-Amz-Signature=b773c2278f8caf3a1c6b86be9f3ff341c0392760473a078960e238ca7ecf8f6b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="70%"></img>

- 서블릿 컨테이너: 서블릿들을 담고 있는 거니까 WAS(Tomcat 등)
- 어플리케이션 컨텍스트: Bean을 관리하는 컨테이너
- IoC Container 구조

    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0fadcb59-8e70-4544-81b6-bb2db4c1d6b7/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAT73L2G45L4OHZXGO%2F20191027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20191027T042044Z&X-Amz-Expires=86400&X-Amz-Security-Token=AgoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQCpIW1f3kvxBpB9%2FTttvhQHEgE8Tf5P%2BmWkOQA1U9EKXgIhAP3Q74bSTb0PS8MiubycaEa%2FI9ZYzZHARKkmFI7BfkAhKvwDCGsQABoMMjc0NTY3MTQ5MzcwIgzLbwJ10XpfcRTD7s8q2QPmA30yndFcB1IK%2Fp59Q2q2%2F2mJUJw4HxVnd6ca1GS3%2BAq9UNix4PIONut85ZU1BCy98UFTC6VMlXVFMTQ47XuDaR2HYIlul2QsOewrmQtBSgYbkkcPNpsH92TOxBwUWRqxTVL7uwOwS3VOtWoQYbG0pOGMgahvklvCXzhofSyNRpk0wTJSxCqO9T2ejiyENm%2BCafX79Bsy7%2Bg673Kt8HcdNlyeiGZ%2BbVnW7Z%2BYszfSSnTqT4HSgJIUifP%2Fg1%2FhcY7%2Fs0%2FHH7nL7hfUefPvcpi6CH80sVPZym9wtGUIQFruYpnW%2FN6SEPFqZpjGvwmS7WM551w271Z%2BsvoQhuqE8VGMZPbOh9FJCxGKxu81lRkXyAgLj9gCsKrQeTahK7AtDxVKmRxdKuAcfmH0%2FSFWuWi9fpQuq6xoI5EJ4pcsJ4vq66H0BRZmqE%2FcGVsS9Cf%2BuakgLVb6CeVBEYpwRyPrMi6PSlwj6fd1yIiP8pFmLIEe7Q6zllg3B7UgUBfoivK27DZ5Tb0H2lA2TBmsY%2F4actDEplFa1%2FtxIbPAXrqLHMyYFH9ALnuf%2FZOc%2FKvq4ObK7hOF7Z6yBhG7l1YBTqqe1tzB0EX0Y%2BmGnVDQZniu%2FixCGTUe2uCcmIe4OTDN5tPtBTqzAb6NTJLSOM9oA8ruPfgg7eU2EdUCADlW91q6ROVNIV7wbN5uvWsWVQExHPMriVeyIAI%2BKWGhpG8vzrue0YVPqsm4%2FJntrn7yktDf5oWk177jtu9EJMWeHht3U0HYjp4T54zD%2FC0tTtgeG2dsYEeUDV2gdwFsRoEj5WNeHYCDLa8e1et9nlzFCJOA%2FqLmDCjBF4fJMWx8m%2F3WBAY6oRVe2qi1GIt8msdN2%2Fq4nk9dVo2T08Ul&X-Amz-Signature=a88363066d3005a76f3f87be7e1a6361359b4ad472b2cf1d7e444ea938ff15da&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="90%"></img>
    
    Spring이 제공하는 대표적인 DI Container는 BeanFactory와 Application Context가 있다~

Spring.io Docs 찾아보면 된다

## 애너테이션

애너테이션은 기능을 하지 않는다~

Annotation Processing

### 사용해 본 애너테이션은 뭐가 있었지

- @Controller, @Service, @Repository, @Component
- @Configuration, @Bean
- @Entity, @Lob, @Embbed
- @ModelAttribute
- @ExceptionHandler

등등...

---

# 테스트

## 인수 테스트(AT)

*ATDD는 인수 테스트 주도 개발 아닌감?*

### TDD vs. ATDD

1. TDD
    - 레이어별로, 실행 단위별로 나눠서 모듈을 테스트 하곤 하지
2. ATDD
    - 클라이언트와 동일한 환경에서 프로그램이 기대하는 흐름대로 잘 작동하는지 테스트 하곤 하지
    - TDD도 하는데 이걸 왜 하냐
        - TDD는 모듈을 테스트하기 때문에 사용자의 사용 환경까진 고려하지 못함

            ex) http method 같은 거?

### 테스트를 작성하면서 종종 이런 생각을 했어…

→ 내가 테스트를 짜는가 테스트가 나를 짜는가…

→ 그런 건 ATDD로 한 게 아니라 만들고 테스트했던 게 아닐까..? (딩동댕)

→ 모든 걸 테스트 주도 방식으로 하려니까 익숙하지 않아서인지 루틴스러운 지겨움이 있따..

→ 단순히 커버리지만을 위한 맹목적인 테스트가 과연 좋은 테스트인가

## Mock Test

### **목을 왜 쓰지**

- 테스트 환경을 일일이 설정하는 것은 테스트 복잡도를 높인다
- 테스트하고 싶은 스코프를 제한하기 위해서
- 데이터베이스 같이 프로덕션의 상태에 영향을 끼칠 수 있는 요소를 최소화한다

### **고민했던 것**

→ 테스트에서 회원가입이나 로그인처럼 선행되어야 하는 단계를 어떻게 깔끔하게 고칠 수 있을까

- 테스트도 추상화할 수 있다
- template 패턴 사용해서 선행 작업 처리
- Mock을 활용해 볼 수도 있다

---

# JPA

### ORM(Object-Relational Mapping)

객체와 관계형 데이터베이스를 매핑하여 DB 쿼리를 손쉽게 도와주는 프레임워크

### Hibernate vs. JPA

- JPA: ORM 표준 기술, 인터페이스
- Hibernate: ORM 표준 기술을 이용한 구현체 중 하나

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/636ffc33-bfdc-419b-a350-8f33add13d5d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAT73L2G45IE5BHKPO%2F20191027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20191027T042228Z&X-Amz-Expires=86400&X-Amz-Security-Token=AgoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJHMEUCIGw%2BYjYpTvxgkS%2B7ySoFPs1YllbcFeFOOs8hB%2BeuxzjHAiEA3QNi2DJnRQlxlw65Zx7U77Emy8eeGeaol7Wg%2FTM2I50q%2FAMIaxAAGgwyNzQ1NjcxNDkzNzAiDEUe2FvBMBQRyZPqpyrZA4qEb2yLrfO663G7RP5MKBNeDkUtZhuYFg5PK%2FB0xlzCq4EB8e5bUMia5Cm9wWzV9gTpcmfggtVT%2BkyUi3Sgg7921srNiSVNU9fYtaxmMcOH2rMMxCKI51bfGMRzUnsUVAIuo1UOv94EMAqL6Ffh3YX7Geb1p6naWnHMwS5OIE4N8gbKtrWpnenYYLbgoeKzPPxz%2BXvCPNk5JBCtVLxEVg7gLOuipKMWwmA70bjq0H5tCnKB%2BB0Uz8LQcZ99PAQsySL%2BaDLeGcpjrnfyZ5jS0nhyWl9mwoZRYxz7Q8HwYde6YIO6HOKX1YbbS9RkQvU%2Fe0xgTd%2FJcUk4kgz1aYJTaaiCBMsxeY95qT1SmE%2FLwpRMZ8AE35WvfdLLmZlohIv%2Fy73NRNYrLix3UmV5aHOVQcpFFKUFuP9rJwRxba3bziphp9h1VjnG3b%2BqF%2FnXuxCtke11OgLSnhRgVm6QeWNzh8TmjC1HLYOiLjP9d3MgmPUTnLENUE6mi1w%2FtNiACBRckbqQnoiSbjvZD9VSwFLs%2Fd0KRN0YOZ1jKvlercY%2F7y3ek%2BfH5v7%2B22lNa6u%2F%2B5qzSpQudOOK2omi5%2BPCEsMYbV3lC4ratk75ukcN3A6BgevGgozzVl858ReNML3w0%2B0FOrQBrsaGFlhmpupK3mS3NedqzsbdsttVyK9jd6%2B6J4lzsSfZq3WAMaStUWIzu0sruvYwpLf4n2xF1tz4KM35DvZXArhqgNMZjXk0p%2Bn%2B8kq2L%2Bf513uUmsI2FkmUJB5R7s%2FvhawdWh%2BLg%2BIirFBhrdeem8dk5uBr5JOEn2pD9MC617f1iSzVlwc2kA%2FbIOTqxXXm6xa0NjP%2BLxNi7prTq%2BVsiw5ZtFue5CsPXLB1VKExKfJGApPg&X-Amz-Signature=7925de5f709502bcc2e6f39d25be929f0d353d10f81a6b20cf6f0c3f6246f541&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22" width="80%"></img>

## 어려웠던 것들

- 양방향/단방향이 적절한 경우를 구분하는 게 어려웠음
- JPA의 지연로딩 때문에 프록시 객체가 던져져서 equals 메서드에서 동등성 확인을 하는데 null값과 비교돼서

    → 로깅 라이브러리인 Slf4j 사용을 시작했다.

    → 디버깅을 백분 활용하자

    ⇒ 프록시 객체를 비교할 때는 equals() 재정의를 잘 하자..

### 엔티티 연관관계

1. OneToOne
2. OneToMany
3. ManyToOne
4. ManyToMany
- 양방향(mappedBy 꼭필요), 단방향

## 영속성 관리

### EntityManager

엔티티를 저장하고, 수정하고, 삭제하고, 조회하는 등 엔티티와 관련된 모든 일을 처리하는 관리자.

### 엔티티 생명주기

- 비영속
- 영속
- 준영속
- 삭제

---

# 그 외

## URI API

깔끔한 API URI 디자인을 위해 다음 두 가지 항목을 기억하자

1. URI는 정보의 자원을 표현해야 한다.

    `/article/5`

2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.

    `~~/update` 이거 아님~~

    PUT `/article/5/comment/2`

## ajax 왜 쓰지..?

- 클라이언트 입장
    - 페이지를 완전히 재로딩하지 않아도
- 서버 입장
    - 필요한 데이터만 요청받아 넘기니까 부하가 적지

## 동일성과 동등성

- 동일성

    두 개의 오브젝트가 완전히 같을 경우 → `==`

- 동등성

    두 오브젝트가 같은 정보를 갖고 있을 경우 → `equals()`

---

# **미니 프로젝트**

## 깃허브를 이용한 첫 협업

1. 깃허브를 적극적으로 이용해서 협업을 해보았다
2. Git flow
3. 칸반보드를 활용한 일정 관리
4. 버전 관리

## 팀 규칙, 문화를 스스로 만들어가자

1. 인상 깊은 점
2. 아쉬웠던 점
