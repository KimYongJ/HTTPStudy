# 5. HTTP 메서드 활용(동적데이터 조회, HTML Form 데이터 전송, API전송 설계 예시)

<br/>

## 클라이언트에서 서버로 데이터 전송

- 방법 1.
  - 쿼리 파라미터를 통한 데이터 전송
  - GET
  - 주로 정렬 필터(검색어)
- 방법 2.
  - 메시지 바디를 통한 데이터 전송
  - POST, PUT, PATCH
  - 회원 가입, 상품 주문, 리소스 등록, 리소스 변경

<br/>

## 클라이언트에서 서버로 데이터 전송하는 4가지 상황

<br/>

### 정적 데이터 조회

- 이미지, 정적 텍스트 문서
- 조회는 GET 사용
- 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능
<img width="831" alt="사진1" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/dd5e7200-85a8-484d-9ff2-51226b842167">

- 단순히 이미지만 받고 싶을 때 url(리소스경로)만 보내면 서버에서 데이터를 클라이언트에게 바로 내려준다.

<br/>

### 동적 데이터 조회

- 주로 검색, 게시판 목록에서 정렬 필터(검색어)
- 조회 결과를 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용함
- 조회는 GET 형태이며 쿼리 파라미터를 사용해 데이터를 전달함 ex) /search?q=hello&hl=ko
<img width="822" alt="사진2" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/d4c8ccf0-25a1-4f20-8805-bc53267fca76">

<br/>

### HTML Form을 통한 데이터 전송

- 방법 1. POST 전송 - 저장

<img width="859" alt="사진3" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/4b359381-b37c-4852-9008-727784c2e218">

  - HTML Form으로 데이터 전송시 method가 POST면 웹브라우저가 메시지 바디에 데이터를 넣는다.

- 방법 2. GET 전송 - 저장

<img width="833" alt="사진4" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/a750384f-a9e3-47de-a10a-558cf1f9f277">

  - HTML Form으로 데이터 전송시 method를 GET으로 할 경우 웹브라우저가 데이터를 쿼리파라미터에 넣는다.
  - 주의 : GET은 조회에서만 사용해야하며 리소스 변경이 발생하는 곳에 사용하면 안된다.

- 방법 3. 파일 데이터 전송

  - HTML Form 전송에서 파일 전송시 쓰는 타입 : multipart/form-data
  - multipart/form-data 작성시 웹브라우저가 그에 맞는 Content-Type을 생성한다.
 <img width="824" alt="사진5" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/4bffc25d-3c7f-47b9-a5b1-68edeb0cff5b">


- HTML Form 데이터 전송 정리

  - HTML Form submit시 POST 전송 ex) 회원 가입, 상품 주문, 데이터 변경
  - Content-Type : application/x-www-form-urlencoded 사용
    - from의 내용을 메시지 바디를 통해서 전송(key=value, 쿼리 파라미터 형식)
    - 전송 데이터를 url encoding 처리 ex) abc강 -> abc%EA%B9%80
  - HTML Form은 GET 전송도 가능
  - Content-Type : multipart/form-data
    - 파일 업로드 같은 바이너리 데이터 전송시 사용
    - 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능
  - HTML Form 전송으 GET, POST만 지원함.

<br/>

### HTTP API를 통한 데이터 전송

- API 데이터 전송은 서버에서 서버끼리 통신할때(백엔드 시스템 통신) 주로 사용한다. (서버끼리 통신할 때는 HTML을 끼지 않기 때문)
- 아이폰, 안드로이드 같은 앱 클라이언트에서 전송할 때도 HTTP API 형식을 사용
- 웹 클라이언트에서 사용
  - HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
  - ex) React, VueJs 같은 웹 클라이언트와 API 통신
- POST, PUT, PATCH : 메시지 바디를 통해 데이터 전송
- GET : 조회, 쿼리 파리미터로 데이터 전달
- Content-Type : application/json을 주로 사용(사실상 표준)
