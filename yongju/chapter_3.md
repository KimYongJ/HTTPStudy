# 3. HTTP 기본(HTTP 특징, HTTP 상태, HTTP지속 연결, HTTP 메시지 구조)

<br/>

## HTTP( Hyper Text Transfer Protocol )의 사용

- 지금은 모든것을 HTTP에 담아서 전송한다
- ex) HTML, TEXT, IMAGE, 음성, 영상, 파일, JSON, XML(API), 거의 모든 형태의 데이터 전송 가능, 서버간 데이터를 주고 받을 때도 대부분 HTTP 사용

- HTTP/0.9 1991년 : GET 메서드만 지원됨, HTTP 헤더 X
- HTTP/1.0 1996년 : 메서드, 헤더 추가
- HTTP/1.1 1997년 : 가장많이 사용되는 버전(RFC2068, RFC2616, RFC7230~7235) 이후 버전들은 성능 개선에 초점이 맞춰짐
- HTTP/2 2015년 : 성능개선
- HTTP/3 진행중 : TCP 대신 UDP를 사용, 성능 개선 중

- TCP : HTTP/1.1, HTTP/2 는 TCP프로토콜 위에서 작동
- UDP : HTTP/3는 UDP 위에서 작동
  <br/><br/><br/>

## HTTP 특징
<br/>

### ① 클라이언트 서버구조

- 설명 : Request Response 구조로 요청과 응답을 주고 받는 구조이며 클라이언트는 서버에 요청을 보내고, 서버는 요청에 대한 결과를 만들어서 응답한다.
- 장점 : 이렇게 함으로써 비즈니스로직과 데이터 같은 것들은 서버에 넣고, 클라이언트는 UI를 그리거나 사용성에 집중하게 된다. 이렇게하면 클라이언트와 서버가 각각 독립적으로 발전할 수 있다. ex) 사용자가 늘어날 경우 클라이언트는 수정할 필요없고, 서버만 개선해도 문제 해결이 가능하다.
<br/>

### ② HTTP는 무상태 프로토콜(Stateless)

- Stateful(상태유지) : 클라이언트와 서버간 작업을 처리하는데 있어서 서버에서 클라이언트가 이전단계에 제공한 값을 저장하고 다음 단계에서도 저장된 값을 활용할 수 있는 상태이다.
- Stateless(무상태) : 서버가 클라이언트의 상태를 보존하지 않는다. 클라이언트의 이전 요청을 서버가 담고 있지 않기 때문에 클라이언트는 요청을 보낼 때 서버가 작업을 처리할 수 있도록 충분한 정보를 전달해 주어야 한다. 서버 개발자는 스테이스리스한 방식으로 설계하는 것을 기억해야한다.
- Stateless(무상태) 장점 : 서버의 확장성이 높다(스케일 아웃), 클라이언트에서 작업을 요청하면 그 요청안에 작업을 처리할 정보가 다 들어이 있기 때문에 서버를 증식하기 수월하다.
- Stateless(무상태) 단점 : 클라이언트가 한번에 보낼 데이터가 많아짐, 상태를 유지해야할 경우(ex로그인 유지 등..)를 최소화 시켜야 한다
<br/>

### ③ 비연결성(connectionless)

- HTTP는 기본적으로 비연결성 모델이다.
- 클라이언트와 서버간 통신을 하며 작업을 처리한 후 서버는 클라이언트와 연결을 끊는다. 이렇게 함으로써 서버입장에서 클라이언트와 연결하는 자원 소모를 최소화 할 수 있다. 이로써 서버는 자원을 매우 효율적으로 사용할 수 있다.

- 비연결성 단점

  - 클라이언트와 처음 통신할 때 TCP/IP 연결을 새로 맺어야한다(3 Way HandShake 시간 추가)
  - 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, CSS, 추가 이미지 등 수많은 자원이 함께 다운로드 된다.

- 비연결성 단점 극복
  - 현재는 HTTP 지속 연결(Persistent Connections)로 문제 해결
  - HTTP/2, HTTP/3에서 더 많은 최적화가 됨
<br/>

### ④ HTTP 메시지

- HTTP 메시지 구조

  - 시작라인(빨강) -> 헤더(노랑) -> 공백 라인(초록)(무조건 써야함) -> 메시지 Body(파랑)
<img width="520" alt="사진1" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/e0997b89-d312-4e38-8f74-649cb3b70fa9">

- HTTP 요청 메시지

  - 위 사진에서 보듯 클라이언트에서 서버로 요청하는 요청 메시지도 HTTP 메시지 구조를 따른다.
  - 요청의 시작라인 예시 : Get /search?q=hello&hl=ko HTTP/1.1
  - 요청의 시작라인 설명 : (①HTTP메서드)(공백)(②요청대상)(③HTTP 버전)
    - ①HTTP 메서드 종류 : GET(리소스 조회), POST(요청 내역 처리), DELETE(리소스 삭제), PUT(리소스 업데이트)
    - ②요청대상 : 절대 경로("/")로 보통 시작하며 쿼리와 합쳐서 사용한다.
    - ③HTTP 버전 : 요청메시지 마직에 HTTP 버전을 넣는다.

- HTTP 응답 메시지

  - 위 사진에서 보듯 서버에서 클라이언트로 응답하는 응답 메시지도 HTTP 메시지 구조를 따른다.
  - 응답 메시지 시작라인 예시 : HTTP/1.1 200 OK
  - 응답 메시지 시작라인 설명 : (HTTP버전)(HTTP 상태 코드)(이유 문구)
    - HTTP 상태코드 : 200(성공), 400(클라이언트 요청 오류), 500(서버 내부 오류)

- HTTP의 응답과 요청 메시지의 헤더

  - HTTP 전송에 필요한 모든 부가정보를 담고 있다.(ex 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청브라우저 정보, 서버 애플리케이션 정보, 캐시 관리 정보 등..)
  - 표준 헤더가 굉장히 많다.
  - 필요시 임의의 헤더 추가 가능

- HTTP의 응답과 요청 메시지의 BODY
  - 실제 전송할 데이터를 담는 부분이며, BYTE로 표현할 수 있는 모든 데이터가 전송된다.(HTML 문서, 이미지, 영상, JSON 등..)
<br/>

### ⑤ 단순함, 확장가능

- HTTP는 단순하다. 스펙도 읽어 볼 수 있음
- HTTP 메시지도 단순하다.
- HTTP는 표준 기술은 단순하지만 확장 가능한 기술
<br/><br/><br/>

## 정리

- HTTP 메시지에 모든것을 전송할 수 있다.
- HTTP/1.1이 많이 사용하기 때문에 1.1 기준의 학습을 권장
- 클라이언트와 서버는 무상태 프로토콜을 사용한다.
- HTTP 메시지는 시작라인, 헤더, 공백, Body로 구조화 되어 있다.
- HTTP는 단순하며 확장 가능하다.