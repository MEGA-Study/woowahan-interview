# HTTP Status, Http Method

## HTTP Status

모든 상태코드를 다 외우고 다닐순 없으니, 우리가 미션 하면서 겪어봤던 코드들만 나열하겠습니다.

- 200 OK
  - 요청 성공
- 201 Created
  - 생성됨
- 204 No Content
  - 요청을 성공했지만, 제공할 내용이 없음
- 206 Partial Content
  - 요청이 일부만 성공함
- 301 Moved Permanently
  - 요청한 URI가 옮겨짐, 새로운 URI가 응답에 주어진다.
- 302 Found
  - 요청한 리소스의 URI가 일시적으로 변경되었음을 의미합니다. 클라이언트는 다음의 요청도 동일한 URI로 해야합니다.
- 304 Not Modified
  - 캐시를 목적으로 사용됩니다. 캐시에서 꺼내서 사용할 수 있다.
- 307 Temporary Redirect
  - 302와 동일, 사용자 에이전트가 HTTP Method를 동일하게 사용해야 함.
- 308 Permanent Redirect
  - 301과 동일, HTTP Method를 동일하게 사용해야 함. 그러면 301, 302는 메소드를 바꿔도 된다는 건가?
- 400 Bad Request
  - 잘못된 문법으로 인하여 서버가 요청을 이해할 수 없음
- 401 Unauthorized
  - "비인증(unauthenticated)"을 의미합니다. 요청한 응답을 받기위해 인증이 필요합니다.
- 403 Forbidden
  - 401과 같지만, 403은 서버가 클라이언트가 누구인지 알고 있다.
- 404 Not Found
  - 서버는 요청받은 리소스를 찾을 수 없습니다.
- 405 Method Not Allowed
  - 서버에서 메소드가 지원되지 않는다. (GET, HEAD는 필수 메소드다)
- 500 Internal Server Error
  - 서버가 처리 방법을 모르는 상황과 마주쳤다.
- 502 Bad Gateway
  - 서버가 요청을 처리하는 데 필요한 응답을 얻기 위해 게이트웨이로 작업하는 동안 잘못된 응답을 수신했음을 의미한다.
  - 서로 다른 프로토콜을 연결해주는 장치(GATEWAY)가 잘못된 프로토콜을 연결하거나, 어느 쪽 프로토콜에 문제가 있어 통신이 제대로 되지 않을 때 출력되는 코드.

## HTTP Method

- GET
  - Request Body X
  - Response Body O
  - 서버의 상태를 변경 시키지 않음(Read-only)
  - 같은 요청은 언제나 같은 결과를 낸다. (idempotent)
  - Response Cacheable
  - HTML Form에서 허용
- HEAD
  - GET과 같지만 Response Body가 없다.
  - 실제 문서를 요청하는 것이 아니라 문서 정보를 요청
- POST
  - 데이터를 서버로 보낸다.
  - Request body는 content-type에 따라 달라진다.
  - POST는 수행할 때마다 계속 수행된다.(POST article 하면 계속 글 써진다.)
- PUT
  - 멱등성을 가진다. (여러번 호출해도 항상 같은 결과)
  - 데이터를 서버로 보낸다.
  - 새로운 데이터를 만들거나 업데이트한다.
- DELETE
  - 특정 리소스를 삭제한다.
  - 정상 Response로 202(Accepted), 204(No Content), 200(OK)
- CONNECT
  - 목적 리소스로 식별되는 서버로의 터널을 맺습니다.?
  - 프록시 기능 요청시 사용합니다.
- OPTIONS
  - 타겟 리소스의 커뮤니케이션 옵션을 나타낸다.(지원되는 메소드 종류를 확인합니다.)
- TRACE
  - 목적 리소스의 경로를 따라 메시지 loop-back 테스트를 한다.
  - 요청 리소스가 수신되는 경로를 보여줌
- PATCH
  - 리소스의 부분만을 수정하는 데 쓰입니다.?

method | GET | HEAD | POST | PUT | DELETE
--- | --- | --- | --- | --- | ---
Request Body | X | X | O | O | ?
Response Body | O | X | O | X | ?
Safe | O | O | X | X | X
Idempotent | O | O | X | O | O
Cacheable | O | O | O | X | X
HTML Form | O | X | O | X | X

### 참고자료

- [https://developer.mozilla.org/ko/docs/Web/HTTP/Status](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
- [https://developer.mozilla.org/ko/docs/Web/HTTP/Methods](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)
- [http://www.ktword.co.kr/abbr_view.php?m_temp1=3791](http://www.ktword.co.kr/abbr_view.php?m_temp1=3791)
