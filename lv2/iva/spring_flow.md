<img width="581" alt="스크린샷 2019-10-21 오후 10 16 49" src="https://user-images.githubusercontent.com/30451129/67225144-a4806000-f46d-11e9-8d45-41ab43097a05.png">


## Spring이 클라이언트 요청을 처리하는 과정

1. 사용자 요청은 `DispatcherSerlvet`이 제일 먼저 받는다.
    - 웹 어플리케이션은 사용자 요청은 tomcat과 같은 서버(WAS)가 설정된 포트번호로 서버 소켓을 열면 웹 서버가 띄어져 있는 컴퓨터의 IP 주소 + 포트번호로 들어오게 된다.
    - WAS는 그 위에서 사용되는 프레임워크인 Spring에게 해당 요청을 전달하는데 제일 먼저 그 요청을 받는 녀석이 `DispatcherServlet` 이다.

2. `DispatcherServlet` 은 받은 요청을 `HandlerMapping` 에게 전달하여 처리할 핸들러를 반환 받는다.

    Spring에서는 요청을 처리하는 녀석을 핸들러(Handler)라고 이야기한다.

3. 핸들러를 반환받으면 핸들러 타입(`HandlerExecutionChain`)을 확인하고 해당 핸들러를 처리할 수 있는 `HandlerAdapter` 를 찾아 처리를 요청한다.

- 여기서 Controller의 API 로직이 실행된다.
- `HandlerAdapter` 가 존재하는 이유
    - `HandlerMapping` 이 반환하는 타입이 항상 동일하지 않기 때문
    - 위에서 핸들러 타입이 `HandlerExecutionChain` 이라고 이야기 했지만 실제 이 클래스 내부를 보면 필드로 `Object handler` 를 두고 있다.
      ```
        public class HandlerExecutionChain { 
        	...
        	private final	Object handler;
        	...
        
        	/**
        	 * Return the handler object to execute.
        	 */
        	public Object getHandler() {
        		return this.handler;
        	}
        }
      ```
    - `HandlerMapping` 이 반환하는 핸들러를 `HandlerAdapter`에게 넘겨줄 때 아래와 같이 넘겨준다.

            public class DispatcherServlet extends FrameworkServlet {
            	...
            
            	// Determine handler adapter for the current request.
            	HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
            	
            	...
            }

    - **그래서 해당 요청을 처리할 수 있는 어댑터를 만들어 요청을 처리할 로직을 실행시키기 위해 `HandlerAdapter` 가 존재한다.**

4. `HandlerAdapter`가 요청을 처리한 후 반환 값을 가지고 결과를 어떤 뷰로 보여줄 것인 결정하기 위해 `ViewResolver` 에게 넘겨준다.

- 결과를 보여줄 `view`를 찾아준다.

5. 반환받은 `view` 로 응답을 생성하고 클라이언트에게 전달한다.

### 갑자기 궁금한 점... 그러면 static 파일은 누가 처리할까?

> 웹 서버(WS)단에서 처리하는 걸로 알고 있는데...그럼 Apache가 처리하는 것인가?

- 만약 서블릿 필터가 없다면 해당 요청은 `DispatcherServlet`까지 들어온다.

<img width="1075" alt="스크린샷 2019-10-22 오전 12 39 10" src="https://user-images.githubusercontent.com/30451129/67225046-76028500-f46d-11e9-9be6-99a95741c9f3.png">

- `DispatcherServlet`이 들어온 요청을 처리할 핸들러를(`HandlerMapping`) `getHandler()` 라는 메서드로 찾는다.
- Spring은 기본적으로 총 5개의 `HandlerMapping` 을 등록해놓는데 그중 맨 마지막인 `SimpleUrlHandlerMapping` 에서 모든 요청을 처리한다. **`"/**"`**
- `SimpleUrlHandlerMapping`에서 아래와 같은 경로로 요청 파일이 있는지 확인하고 없을 시 에러를 발생시킨다.

<img width="550" alt="스크린샷 2019-10-22 오전 12 54 24" src="https://user-images.githubusercontent.com/30451129/67225026-6b47f000-f46d-11e9-8537-022a12d7d71f.png">
