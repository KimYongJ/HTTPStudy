# 7. HTTP 헤더(표현헤더, 콘텐츠 네고시에이션(협상)헤더들, 전송방식 관련 헤더, 기타헤더, 특별한 헤더, 인증 관련 헤더, 쿠키와 관련된 것)

<br/>

# 목차
- [HTTP 헤더 용도](#HTTP-헤더-용도)
- [지금은 폐기된 RFC2616 스팩 설명](#지금은-폐기된-RFC2616-스팩-설명)
- [현재 RFC7230~7235 등장](#현재-rfc72307235-등장)
- [RFC723x 헤더의 '표현'과 관련된 헤더들](#RFC723x-헤더의-표현과-관련된-헤더들)
- [콘텐츠 네고시에이션(협상)헤더](#콘텐츠-네고시에이션협상헤더)
- [전송방식 관련 헤더](#전송방식-관련-헤더)
- [기타 헤더들](#기타-헤더들)
- [특별한 헤더들](#특별한-헤더들)
- [인증 관련 헤더](#인증-관련-헤더)
- [쿠키 관련 헤더](#쿠키-관련-헤더)

<br/>
  
## HTTP 헤더 용도

- HTTP 전송에 필요한 모든 부가정보를 헤더에 담는다.

  - 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리정보 등..

- 표준 헤더가 많으며 필요시 임의의 헤더 추가가 가능하다.
  - 참고 : https://en.wikipedia.org/wiki/List_of_HTTP_header_fields

<br/>

## HTTP 헤더 분류

### 지금은 폐기된 RFC2616 스팩 설명

- 2014년 RFC7230~7235의 등장으로 RFC2616은 폐기되었다. 아래 설명은 폐기된 RFC2616 설명이다.

  - General 헤더 : 메시지 전체에 적용되는 정보이다. 요청,응답 구분 없이 메시지 전체 적용되는 헤더
  - Request 헤더 : 요청보낼 때 들어가는 헤더이다. (예를 들어 User-Agent 정보 등..)
  - Response 헤더 : 응답에 들어가는 메시지 이다.(예를 들어 Server 정보가 아파치이다 등..)
  - Entity 헤더 : 메시지 바디에 들어가는 내용과 관련된 정보를 담는 헤더이다 (Content-Type 이나 Content-Length 등..) 엔티티 헤더는 엔티티 본문의 데이터를 해석할 수 있는 정보를 제공한다.(데이터 유형(html, json), 데이터 길이, 압축 정보 등등..)
  - 메시지 본문 안에 엔티티 본문이 들어가있다. 엔티티 본문은 요청과 응답에서 전달할 실제 데이터이다.
   <img width="668" alt="사진1" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/15f18c8d-f720-4606-ae3b-b21590381a11">

<br/>

### 현재 RFC7230~7235 등장

- RFC723x 에서 변화한 내용

  - 엔티티(Entity) -> 표현(Representation)으로 용어 변경
  - 표현(Representation) = 표현메타데이터(representation Metadata) + 표현데이터(Representation Data)
  - 메시지 본문(Message Body)을 통해 표현 데이터를 전달한다.
  - 메시지 본문 = 페이로드(PayLoad)
  - 표현은 요청이나 응답에서 전달할 실제 데이터
  - 표현 헤더는 표현 데이터를 해석할 수 있는 정보를 제공한다.(데이터 유형(html, json), 데이터 길이, 압축 정보 등등)
   <img width="663" alt="사진2" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/69249b88-967a-43d0-9a91-8c919282d5a3">

<br/>

### RFC723x 헤더의 표현과 관련된 헤더들

- 참고 : 표현 헤더는 전송과 응답에서 다 사용 가능하다.
- Content-Type : 표현 데이터의 형식을 담는다.
  - 미디어 타입, 문자 인코딩 관련 정보를 넣는다 ex) Content-Type : text/html;charset=UTF-8 혹은 Content-Type : application/json
- Content-Encoding : 표현 데이터의 압축 방식을 명시한다. 데이터를 전달하는 곳에서 압축 후 인코딩 헤더를 추가하며 읽는 쪽에서는 인코딩 헤더의 정보로 압축을 해제 한다. ex) Content-Encoding : gzip|deflate|identity(압축안함을의미)
- Content-Language : 표현 데이터의 자연 언어표시(한국어인지 영어인지 등..) ex) Content-Language: ko
- Content-Length : 표현 데이터의 길이를 바이트 단위로 명시한다. Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안된다.

<br/>

### 콘텐츠 네고시에이션(협상)헤더

- 클라이언트가 원하는 표현 방식을 서버에게 요청하는 것으로 서버는 최대한 클라이언트가 원하는 표현을 만들어 준다.
- 협상헤더는 요청시에만 사용한다.
- Accept : 클라이언트가 선호하는 미디어 타입을 서버가 갖고있을 경우 요청
- Accept-Charset : 클라이언트가 선호하는 문자 인코딩을 서버가 갖고있을 경우 요청
- Accept-Encoding : 클라이언트가 선호하는 압축 인코딩을 서버가 갖고있을 경우 요청
- Accept-Language : 클라이언트가 선호하는 자연언어(한글, 영어)를 서버가 갖고있을 경우 요청
  - Accept-Language 적용 전
   <img width="829" alt="사진4" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/f38dfa99-6ec6-4ee4-b0eb-36dbed6472be">

  - Accept-Language 적용 후
   <img width="805" alt="사진3" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/2c061d2d-f8e4-424f-8adb-ad0bbb881ca6">

  - Accept-Language를 요청했으나 해당 언어가 없는 경우
   <img width="800" alt="사진5" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/8075406a-d8b1-4150-8f63-16877aa78d1c">

- 협상과 우선순위
  - 위의 예시처럼 독일어, 영어만 지원하고 한국어가 없을 경우 언어에 대한 우선순위를 헤더에 실어 보낼 수 있다.
  - 우선순위는 q(Quality Values)로 표현하며 0~1 사이 값으로 표현하며 1이 가장 큰값이다. 생략하면 1이 default값이다.
  - ex) Accept-Language : ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7;
    - 해석 : 콤마(,)로 각각 구분, 언어코드(ex ko)와 세미콜론(;), q를 하나로 묶어 해석한다.
  - 아래 사진은 우선순위를 실어 보낸 후 응답 예시이다.
  <img width="801" alt="사진6" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/d37c00f5-f0aa-4644-9311-bc3b17f309a2">

- 협상과 우선순의 조건
  - 우선순위를 적을 때 구체적으로 적은 것이 우선한다.
  - ex) Accept: text/\*, text/plain, text/plain;format=flowed 이 있을 때 가장 구체적으로 적은 text/plain;format=flowed 가 가장 우선한다.

<br/>

### 전송방식 관련 헤더

- 단순 전송 : 메시지 바디 컨텍트에 대한 길이를 알 수 있을 때 사용하며 Content-Length 값을 명시한다.
- 압축 전송 : Content-Encoding을 추가로 명시해야 한다.
- 분할 전송 : Transfer-Encoding: chunked 라는 것을 명시하여 데이터를 전송한다. 예를 들어 큰 데이터를 보낼 때 5바이트씩 잘라 보내는 등 부분을 먼저 보내 클라이언트가 특정 부분 부분을 먼저 받아보는 것이다. 분할 전송시 주의할 점은 Content-Length값을 명시하면 안된다.
- 원하는 범위 전송 : Content-Range: bytes 1001-2000 / 2000 형태로 명시하며 특정 바이트 부분을 명시하여 원하는 부분을 전송하는 것이다.

<br/>

### 기타 헤더들

- 단순한 정보성 헤더들이다.
- From : 유저 에이전트의 이메일 정보로 자주 사용하진 않으며 검색 엔진 같은 곳에서 주로 사용되며 요청에서 사용한다.
- Referer
  - 가장 많이 쓰이는 헤더
  - 현재 요청된 페이지의 이전 웹페이지 주소를 의미한다.
  - A페이지에서 B페이지를 요청할 때 Referer: A 를 포함해서 요청하며 이렇게 하여 Referer를 사용해 유입 경로를 분석할 수 있다.
  - Request Header에 실어 보낸다.
- User-Agent
  - 클라이언트의 애플리케이션 정보이다(웹 브라우저 정보 등)
  - 어떤 종류의 브라우저에서 장애가 발생하는지 파악할 수 있다.
  - 통계 정보를 만들 수 있다.
  - Request Header에 실어 보낸다.
- Server
  - 요청을 처리하는 Origin(실제 응답 서버) 서버의 소프트웨어 정보
  - Response Header에 실어 보낸다.
  - ex) server : nginx
- Date : 메시지가 발생한 날짜와 시간이며 응답에서 사용함

<br/>

### 특별한 헤더들

- Host 헤더

  - Host 헤더는 필수 값이다. 요청에서 사용함.
  - 하나의 서버가 여러 도메인을 처리해야 할 때
  - 하나의 IP주소에 여러 도메인이 적용되어 있을 때
  - 아래 사진은 하나의 서버에서 여러개의 어플리케이션을 실행할 때 Host 값으로 어떤 어플리케이션을 실행할지 찾는 예시이다.
   <img width="949" alt="사진7" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/4908a8de-994f-40e8-8e20-16471a116521">

- Location 헤더

  - 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
  - 201 (Created)에서 사용될 경우 Location 헤더는 요청에 의해 생성된 리소스 URI를 의미한다.
  - 3xx (Redirection)에서 사용될 경우 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킨다.
  - [ [리디렉션 설명] ](https://github.com/KimYongJ/HTTPStudy/blob/main/yongju/chapter_6.md)

- Retry-After
  - 이 헤더는 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간을 알려주는 역할이다.
  - 503 (Service Unavailable) : 서비스가 언제까지 불능인지 알려줄 수 있음
  - ex) Retry-After: Fri, 31 Dec 1999 23:59:59 GMT(날짜 표기)
    Retry-After: 120 (초단위 표기)

<br/>

### 인증 관련 헤더

- Authorization
  - 클라이언트 인증 정보를 서버에 전달하는 헤더
  - ex) Authorization: Basic xxxxxxxxxxxxx
- WWW-Authenticate
  - 접근은 했으나 인증에 문제가있을 경우 해당 헤더를 사용해 필요한 인증방법을 알려줄 수 있다. 401 Unauthorized 응답과 함께 사용한다.
  - ex) WWW-Authenticate: Newauth realm="apps", type=1, title="Login to \"apps\"",Basic realm="simple"

<br/>

### 쿠키 관련 헤더

- 쿠키
  - 쿠키 사용이유 : 클라이언트와 서버가 요청과 응답을 주고 받은 후 연결이 끊어진다. 그렇기 때문에 서버는 클라이언트가 요청을 보낼 때 마다 이전 요청을 기억하지 못한다.(무상태 프로토콜) 이 때 사용자 정보등 식별자를 쿠키에 담아 통신시 확인함으로 사용자를 식별하도록 하였다. 모든 요청에 쿠키 정보는 자동으로 포함된다. 아래 사진은 로그인시 쿠키를 서버에서 클라이언트로 저장하는 예시이다.
    <img width="893" alt="사진8" src="https://github.com/KimYongJ/HTTPStudy/assets/106525587/5268456e-3773-43a4-bb54-d4fb4dfd6979">

  - 쿠키의 쓰임 : 사용자 로그인 세션관리 , 광고 정보 트래킹등에 사용된다.
  - 쿠키의 특징 : 추가 정보를 담기 때문에 네트워크 트래픽이 추가된다. 보안에 민감한 데이터는 쿠키에 저장하면 안되며 최소한의 정보만 저장해야한다(세션 id, 인증 토큰).
  - Set-Cookie : 서버에서 클라이언트로 쿠키 전달(응답시 사용)
  - Cookie : 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달 한다.
  - 쿠키 생명주기
    - 쿠키의 유효시간을 정할 수 있다. (expires 혹은 max-age 사용)
    - Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT(만료일이 되면 쿠키 자동 삭제)
    - Set-Cookie: max-age=3600 (3600초 후 삭제, 0 혹은 음수 지정시 쿠키 자동 삭제)
    - 세션 쿠키 : 만료 날짜를 생략하면 브라우저 종료시 까지만 유지된다.
    - 영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지된다.
  - 쿠키에 도메인(Domain) 적기
    - domain=example.org 라고 적게되면 명시한 문서 기준 도메인과 서브 도메인 포함 쿠키에 접근할 수 있다.
    - 도메인 생략시 현재 문서 기준 도메인만 쿠키에 접근할 수 있다. 서브 도메인은 쿠키에 접근이 불가하다.
  - 쿠키에 경로(Path) 적기
    - 쿠키에 경로를 포함하면 해당 경로를 포함한 하위 페이지만 쿠키에 접근이 가능하다.
    - 보통 경로는 루트(Root)로 지정한다. 일반적으로 한 도메인안에서 쿠키를 다 접근할 수 있어야 하기 때문이다.
    - ex) path=/home 이라고 명시할 경우 /home/room1 , /home/room2 에는 접근이 가능하고 /house 경로가 있다면 이 경로는 쿠키에 접근이 불가능하다.
  - 쿠키에 보안 적용하기 ( Secure, HttpOnly, SameSite )
    - Secure
      - 쿠키는 원래 http, https를 구분하지 않고 전송하지만 Secure 옵션을 적용하면 https인 경우만 쿠키를 전송한다.
    - HttpOnly
      - XSS 공격 방지를 위한 옵션이며 해당 옵션을 사용하면 자바스크립트에서 쿠키에 접근이 불가능하다. HTTP 전송에서만 사용 가능한 옵션이다.
    - SameSite
      - XSRF 공격을 방지하기 위한 옵션이며 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송이 가능하다.
