# 6. HTTP 상태코드(200,300,400,500)

<br/>

## 클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능

<br/>

### 1xx (Informational) : 요청이 수신되어 처리 중(거의 사용되지 않는다.)

<br/>

### 2xx (Successful) : 요청 정상 처리됨

- 200 OK : 정상 완료
- 201 Created : 클라이언트 요청에 의해 서버에서 리소스 등을 생성했을 때
- 202 Accepted : 요청이 접수되었으나 처리가 완료되지 않았음을 의미한다. 주로 배치 처리 같은 곳에서 사용한다. (예시 : 요청 접수 후 1시간 뒤에 배치 프로세스가 요청을 처리함 등..)
- 204 No Content : 서버가 요청을 성공적으로 수행했지만, 응답 페이지로드 본문에 보낼 데이터가 없을 경우이다. 결과 내용이 없어도 2xx란 값만으로 성공을 인식할 수 있다.

<br/>

### 3xx (Redirection) : 요청을 완료하려면 유저 에이전트의 추가 행동이 필요

- 리다이렉션 의미 : 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동함(리다이렉트)
- 리다이렉션 종류
  - 영구 리다이렉션 : 특정 리소스의 URI가 영구적으로 이동, 원래의 URL을 사용하지 않으며 검색 엔진 등에서도 변경을 인지함
    - 301 Moved Permanently : 리다이렉트시 요청 메서드가 POST여도 GET으로 변하고, 본문이 제거될 수 있음
      <img width="871" alt="사진2" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/ba7c6487-e65f-4b1c-8719-294f3a76564f">
    - 308 Permanent Redirect : 301과 기능은 같고, 리다이렉트시 요청 메서드와 본문 유지(처음 POST를 보내면 리다이렉트도 POST로 됨), 실무에서 거의 안씀
      <img width="871" alt="사진3" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/8c395656-1e44-4674-9b33-544829a62d0a">

  - 일시 리다이렉션 : 일시적인 변경, 잠깐 이동시킬 때 사용함(주문 완료 후 주문 내역 화면으로 이동), 따라서 검색 엔진 등에서 URL을 변경하면 안된다. 실무에서 가장 많이씀
    - 302 Found : 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음, 실무에서 많이 사용함
    - 307 Temporary Redirect : 302와 기능은 같음, 리다이렉트시 요청 메서드와 본문을 반드시 유지해야 한다.(요청 메서드를 변경하면 안된다.)
    - 303 See Other : 302와 기능은 같음, 리다이렉트시 요청 메서드가 GET으로 변경됨
    - 리다이렉션은 언제 쓸까?( PRG : Post / Redirect / Get )
      - POST로 주문 후 웹 브라우저를 새로고침하면? 중복주문 될 수 있다.
      - 아래 사진은 PRG 사용 전의 흐름이다.
        <img width="1065" alt="사진4" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/f26d1c7b-69dd-4200-97a0-d26844c6aa0b">

      - 위와 같은 문제를 해결하기 위해서 POST로 주문후에 주문 결과 화면을 GET 메서드로 리다이렉트하고, 새로고침해도 결과 화면을 GET으로 조회하도록한다. 즉 중복 주문 대신에 결과 화면만 GET으로 다시 요청토록함. 아래 사진은 PRG적용 후의 흐름이다.
        <img width="1072" alt="사진5" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/bd463c66-8d16-41aa-ad61-066c5f7bffd2">

      - URL이 이미 POST에서 GET으로 리다이렉트되어 새로고침해도 GET으로 결과 화면만 조회된다.
  - 특수 리다이렉션 : 결과 대신 캐시를 사용
    - 304 Not Modified : 캐시를 목적으로 사용하며 클라이언트에게 리소스가 수정되지 않았음을 알려준다. 따라서 클라이언트는 로컬 PC에 저장된 캐시를 재사용한다.(캐시로 리다이렉트 한다.) 304 응답은 응답에 메시지 바디를 포함하면 안된다(로컬 캐시를 사용해야 하기 때문에). 조건부 GET, HEAD 요청시 사용한다.
  - 정리
    - 처음 302 스펙의 의도는 HTTP 메서드를 유지하는 것이였지만, 웹 브라우저들이 대부분 메서드를 GET으로 바꾸어 버리기 때문에 모호한 302대신 명확한 303, 307, 301, 308등이 등장함
    - 307, 303을 권장하나 현실적으로 이미 많은 애플리케이션 라이브러리가 302를 기본 값으로 사용중이다. 자동 리다이렉션시 GET으로 메서드가 변해도 된다면 302를 사용해도 큰 문제가 없다.
    - 300 Multiple Choices : 거의 안씀
    - 301 Moved Permanently : 영구 리다이렉션 , 클라이언트에 응답할 때 Location에 url을 전달하여 리다이렉트하게 함(아래 사진 예시)
      <img width="867" alt="사진1" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/f79cc8dc-5aff-4194-976e-75ba8ce957d9">

    - 302 Found : 일시적 리다이렉션
    - 303 See Other : 일시적 리다이렉션
    - 304 Not Modified : 특수 리다이렉션
    - 307 Temporary Redirect : 일시적 리다이렉션
    - 308 Permanent Redirect : 영구 리다이렉션

<br/>

### 4xx (Client Error) : 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음

- 오류의 원인이 클라이언트에 있다.
- 중요! 클라이언트가 이미 잘못된 요청, 데이터를 보내고 있기 때문에, 똑같은 재시도가 실패함
- 400 Bad Request : 클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음(요청 구문, 메시지 등등 오류, 클라이언트는 요청 내용을 다시 검토하고 보내야한다. 요청 파라미터가 잘못되었거나, API 스펙이 맞지 않을 때 등이 원인이다.)
- 401 Unauthorized : 클라이언트가 해당 리소스에 대한 인증이 필요함
  - 인증(Authentication) 되지 않음( 본인이 누구인지 확인이 필요한 경우 )
  - 401 오류 발생시 응답에 WWW-Authenticate 헤더와 함께 인증 방법을 설명
- 403 Forbidden : 서버가 요청을 이해했지만 승인을 거부함
  - 주로 인증 자격 증명은 있지만, 접근 권한이 불충분한경우
  - 예) 어드민 등급이 아닌 사용자가 로그인은 했지만, 어드민 등급의 리소스에 접근하는 경우
- 404 Not Found : 요청 리소스가 서버에 없음, 또는 클라이언트가 권한이 부족한 리소스에 접근할 때 해당 리소스를 숨기고 싶을 때

<br/>

### 5xx (Server Error) : 서버 오류, 서버가 정상 요청을 처리하지 못함

- 서버 문제로 오류 발생, 서버에 문제가 있기 때문에 재시도 하면 성공할 수도 있다(복구가 되거나 등등)
- 500 Internal Server Error : 서버 문제로 오류 발생, 애매하면 500으로 오류를 띄우면 된다.
- 503 Service Unavailable : 서비스 이용불가 상태, 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없을 때 사용, Retry-After 헤더 필드로 얼마뒤에 복구되는지 보낼 수도 있다.

<br/>

### 모르는 상태코드가 전달 될 경우 해결 방법

- 클라이언트는 상위 상태코드로 해석해서 처리한다.
- 미래에 새로운 상태 코드가 추가되어도 클라이언트를 변경하지 않아도 됨
- 예시)
  - 299 -> 2xx로 인식 (Successful)
  - 499 -> 4xx로 인식 (Client Error)
  - 599 -> 5xx로 인식 (Server Error)