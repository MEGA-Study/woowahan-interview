클라이언트와 서버가 요청 또는 응답으로 컨텐츠를 전달하기 위한 부가적인 정보를 담는 역할

HTTP 메시지의 역할은 **(Request)클라이언트가 원하는 정보를 서버에게 요청하기 위해 저장**하는 것이고, 그 요청에 대해 **(Response)서버가 처리한 응답 정보를 저장**하는 것.

<br>

**HTTP 메시지 = StartLine + Header +  Body**
<br><br>

### Body

1. Request
    - POST일 때만 사용
    - 클라이언트가 요청하고자 하는 정보를 얻기 위해 필요한 조건들
2. Response
    - 클라이언트가 요청한 데이터

<br><br>
## 그렇다면 헤더는?

요청과 응답에 대한 부가적인 정보들이 담겨 있다

- **헤더의 역할은?**
- 컨텍스트에 따라

<br><br>
## 공통 헤더

요청과 응답에 모두 사용되는 헤더

### **Date**

> Date: Thu, 12 Jul 2018 03:12:27 GMT

### **Connection**

> Connection: keep-alive

Connection은 기본적으로 keep-alive로 되어 있다.

(HTTP/2에서는 아예 사라짐)

### **Cache-Control**

웹 자원을 효율적으로 사용하기 위한 캐싱에 대한 설정 정보를 저장

### **Content-Length**

요청과 응답 메시지의 본문 크기를 바이트 단위로 표시해 준다.

> Content-Length: 52

- 왜 사용하니?

    ⇒ 오류 제어

### **Content-Type**

> Content-Type: text/html; charset=utf-8

컨텐츠의 타입(MIME)과 문자열 인코딩(utf-8 등등)을 명시할 수 있다. Accept 헤더, Accept-Charset 헤더와 대응된다.

### **Content-Language**

사용자의 언어 (컨텐츠의 언어 아님!)

### **Content-Encoding**

> Content-Encoding: gzip, deflate

컨텐츠 압축 방식

응답 컨텐츠를 br, gzip, deflate 등의 알고리즘으로 압축해서 보내면, 브라우저가 알아서 해제해서 사용한다.

컨텐츠 용량이 줄어들기 때문에 요청이나 응답 전송 속도도 빨라지고, 데이터 소모량도 줄어든다.

<br>

## Cache-Control

웹 자원을 효율적으로 사용하기 위한 캐싱에 대한 설정 정보를 저장

보통, 캐싱은 GET에서만 사용

- no-cache
- no-store

- 관련된 헤더

    ### Age

    > Age: 60

    ### Expires

    > Expires: Thu, 26 Jul 2018 07:28:00 GMT

    응답 컨텐츠가 언제 만료되는지를 나타냄

    `Cach-Control`에 `max-age`가 있는 경우에는 무시됨

    ### ETag (of Response)

    > Etag: 12345

    **응답 메시지에서** HTTP 컨텐츠가 바뀌었는지 검사할 수 있는 태그

    **요청 메시지에서** HTTP 컨텐츠가 바뀌었는지 검사할 수 있는 태그

    > If-None_Match: 12345

    ### If-None-Match (of Request)

<br>

### HTTP 1.1에서 캐시를 판단하는 방법

1. 서버 컨텐츠의 내용이 바뀌기 전의 Response ETag 헤더가 12345라 하자. 
2. 클라이언트는 Request 헤더에 `If-None-Match`라는 태그에 12345라는 값을 태워서 서버에 요청을 보낸다.

    → 304 Not Modified

3. 어느 날, 서버의 컨텐츠가 바뀌게 되면 서버의 ETag의 값도 달라진다.
4. 서버는 클라이언트의 `If-None-Match`의 값이 ETag와 다른 것을 알아채고 새로운 컨텐츠를 전송하게 된다.
