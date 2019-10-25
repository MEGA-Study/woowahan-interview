## Cross-Origin Resource Sharing

처음 전송되는 리소스의 도메인과 다른 도메인으로부터 리소스가 요청될 경우 해당 리소스는 cross-origin HTTP 요청에 의해 요청된다.

예를 들어 [http://domain-a.com으로](http://domain-a.com으로)부터 전송되는 HTML 페이지가 `<img src="">` 속성을 통해 [http://domain-b.com/image.jpg](http://domain-b.com/image.jpg)을 요청하는 경우를 이야기한다. 오늘날 많은 웹 페이지들은 CSS 스타일 시트, 이미지, 스크립트 리소스를 각각의 출처로부터 읽어온다.

보안 상의 이유로, 브라우저들은 스크립트 내에서 초기화하는 cross-origin HTTP 요청을 제한한다. `XmlHttpRequest` 은 same-origin 정책을 따르기 때문에 자신과 동일한 도메인으로부터 HTTP 요청을 보내는 것만 가능했다.

The same-origin policy is a critical security mechanism that restrict how a document or script loaded from one origin can interact with a resource form another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.

### cross-site의 보안 문제

- **Cross-Site Scripting (XSS)**
    - 크로스 사이트 스크립팅은 사용자가 입력한 정보를 출력할 때 스크립트가 실행되도록 하는 공격기법이다. 다른 사이트로 어떤 정보를 전송하는 행위가 주로 일어나기 때문에 사이트간 스크립팅이라는 이름을 가지고 있다.
    - 예를 들어, cross-site을 완전히 허용한다면 브라우저를 통해 한 사용자가 개인 정보를 입력할 때, 스크립트 언어를 실행시켜 도메인이 다른 서버(해킹 서버)로 요청을 보내게 되게 될 수 있다. 만약 개인 정보와 같은 중요한 정보가 쿠키에 저장되어 있다면 당연히 다른 서버(해킹 서버)로 같이 보내지기 때문에 보안의 이슈가 발생할 수 있다.

### 그렇다면 어떤 요청이 CORS를 사용할까?

- cross-site 방색 내에서의 `XMLHttpRequest` API 호출
- CSS 내 @font-face에서의 cross-domain 폰트 사용을 위한 웹폰트 사용할 시
- WebGL
- `drawImage` 를 사용해 캔버스에 그려지는 이미지 / 비디오 프레임들
- CSSOM 접근을 위한 스타일시트
- 활성화된 예외 보고를 위한 스크립트

CORS는 웹 서버에게 보안 cross-domain 데이터 전송을 활성화하는 cross-domain 접근 제어권을 부여한다.

[http://foo.example](http://foo.example) 도메인 상의 웹 컨텐츠가 [http://bar.other](http://bar.other) 도메인 상의 컨텐츠를 호출하는 상황에서 주고받는 요청과 응답

![IMG_7E519CEEABC3-1](https://user-images.githubusercontent.com/30451129/67553569-79855d00-f748-11e9-8c87-27e0436150dd.jpeg)

10번째 줄에 있는 `Origin` 는 해당 요청이 'http://foo.example' 도메인에 있는 컨텐츠로부터 온 것이라는 것을 알려주는 중요한 HTTP 요청 헤더다.

13번째부터 22번째 줄은 [http://bar.other](http://bar.other) 도메인 서버에서 온 HTTP 응답이다. 16번째 줄의 `Access- Control-Allow-Origin:*`  는 서버의 리소스가 cross-site 방식 내에서 모든 도메인으로부터 접근 가능하다는 것을 의미한다.

즉, [http://bar.other](http://bar.other) 도메인 서버는 cross-site 방식에 대한 제약이 없게 둔다는 것이다. 만약 `Access- Control-Allow-Origin:http://foo.example` 라고 설정한다면 cross-site 방식을 요청 값 헤더 `Origin` 의 값이 http://foo.example 에 대해서만 허용한다고 설정하는 것이다.
