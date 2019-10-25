# Servlet Container vs. Spring Container(DI Container)

둘다 컨테이너라는 이름을 사용한 것으로 보아 무엇인가를 보관하는 공간인 것 같다.

- Servlet Container(대표적으로 Tomcat)는 서블릿*들의 저장 공간이다.
  - 여러 서블릿들의 생명주기 관리, 멀티쓰레드 지원을 한다.
- Spring MVC도 Servlet Container가 관리하고 있는 Servlet이다.
  - Spring MVC로의 모든 Request, Response는 DispatcherServlet에서 관리한다.

*서블릿: 자바로 웹을 개발하기 위해 정해진 규약? 인터페이스? JPA같은 느낌이다.

## Servlet Container

서블릿 컨테이너는 개발자가 웹서버와 통신하기 위하여 소켓을 생성하고, 특정 포트에 리스닝하고, 스트림을 생성하는 등의 복잡한 일들을 할 필요가 없게 해준다. 컨테이너는 servlet의 생성부터 소멸까지의 일련의 과정(Life Cycle)을 관리한다. 대표적인 Servlet Container가 Tomcat 이다.

1. Http request를 Servlet Container에 보낸다.
2. Servlet Container는 HttpServletRequest, HttpServletResponse 객체를 생성한다.
3. 사용자가 요청한 URL 분석해서 어느 서블릿 요청인지 찾는다.
4. service() 호출 -> doGet() or doPost()
5. HttpServletResponse에 응답을 보냄.

## Spring Container (DI Container)

Spring Container는 Bean의 생명주기를 관리한다.

1. 웹 애플리케이션이 실행되면 Tomcat(WAS)에 의해 web.xml이 loading된다.
2. web.xml에 등록되어 있는 ContextLoaderListener(Java Class)가 생성된다. ContextLoaderListener 클래스는 ServletContextListener 인터페이스를 구현하고 있으며, ApplicationContext를 생성하는 역할을 수행한다.
3. 생성된 ContextLoaderListener는 root-context.xml을 loading한다.
4. root-context.xml에 등록되어 있는 Spring Container가 구동된다. 이 때 개발자가 작성한 비즈니스 로직에 대한 부분과 DAO, VO 객체들이 생성된다.
5. 클라이언트로부터 웹 애플리케이션이 요청이 온다.
6. DispatcherServlet(Servlet)이 생성된다. DispatcherServlet은 FrontController의 역할을 수행한다. 클라이언트로부터 요청 온 메시지를 분석하여 알맞은 PageController에게 전달하고 응답을 받아 요청에 따른 응답을 어떻게 할 지 결정만한다. 실질적은 작업은 PageController에서 이루어지기 때문이다. 이러한 클래스들을 HandlerMapping, ViewResolver 클래스라고 한다.
7. DispatcherServlet은 servlet-context.xml을 loading 한다.
8. 두번째 Spring Container가 구동되며 응답에 맞는 PageController 들이 동작한다. 이 때 첫번째 Spring Container 가 구동되면서 생성된 DAO, VO, ServiceImpl 클래스들과 협업하여 알맞은 작업을 처리하게 된다.

### 참고자료

- [https://minwan1.github.io/2017/10/08/2017-10-08-Spring-Container,Servlet-Container/](https://minwan1.github.io/2017/10/08/2017-10-08-Spring-Container,Servlet-Container/)
- [https://jojoldu.tistory.com/28](https://jojoldu.tistory.com/28)
