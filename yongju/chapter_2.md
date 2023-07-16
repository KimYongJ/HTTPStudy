# 2. URI, 웹브라우저 요청 흐름, HTTP 메시지 전송 흐름
<br/><br/>
## URI

- URI( Uniform Resource Identifier )
  - Uniform : 통일된 방식
  - Resource : 자원을 의미하며 URI로 식별할 수 있는 모든것을 의미한다(제한 없음)
  - Identifier : 다른 항목과 구분하는데 필요한 정보
  - URI는 리소스를 식별하는 통합된 방법
<br/><br/><br/><br/>
## URI의 종류
- URI는 URL과 URN을 포함한다.
![사진1](https://github.com/KimYongJ/HTTPStudy/assets/106525587/b2bb84ec-a054-46b5-a2d1-5e599ebe8371)

<br/><br/><br/><br/>
## URL

- URL( Uniform Resource Locator )
  - Locator라는 이름에서 알 수 있듯이 리소스가 있는 위치를 의미한다.
- URL 분석
  - 예시 : https://www.google.com:443/search?q=yongju
  - 프로토콜 : https
    어떤 방식으로 자원에 접근할 것인가 하는 약속 or 규칙을 의미하며 http, https, ftp 등을 사용할 수 있다.
  - 호스트명 : www.google.com
    도메인명이나, IP 주소를 직접 입력하여 사용할 수 있다.
  - 포트번호 : 443
    포트는 생략 가능하다. 생략시 프로토콜이 http이면 80포트를, https이면 443포트가 자동으로 사용된다.
  - 경로(패스) : /search
    리소스의 경로를 의미하며, 계층적인 구조를 갖는다.
    ex) /home/face.jpg , /members/100
  - 쿼리 파라미터 : q=yongju
    key, value 형태이다.
    쿼리 파리멑, 쿼리스트링으로 불리며 웹 서버에 제공하는 파라미터로 문자 형태로 작성된다.
    물음표(?)로 시작하고, 엔드(&) 기호로 파라미터를 추가할 수 있다.
    ex) ?keyA=valueA&keyB=valueB
  - Fragment
    html 내부에서 북마크 등에 사용되고 서버에 전송하는 정보는 아니다. 자주 사용되지 않음
<br/><br/><br/><br/>
## URN

- URN( Uniform Resource Name )
  - 리소스에 이름을 부여하여 부르는 방식
  - 리소스의 위치(URL)는 변할 수 있지만 리소스의 이름(URN)은 변하지 않는다.
  - URN(리소스 이름)만으로 실제 리소스를 찾을 수 있는 방법이 보편화되어 있지 않기 때문에 잘 사용되지 않는다.
<br/><br/><br/><br/>
## 웹브라우저의 요청 흐름 , HTTP 메시지 전송 흐름
![사진2](https://github.com/KimYongJ/HTTPStudy/assets/106525587/6f66c2f4-934e-4117-8221-133ef07ec507)

1. 웹브라우저에 URL 주소가 입력된다.
2. 웹브라우저에서 DNS에 호스트명을 조회하고 해당 호스트명을한 서버의 IP 주소를 얻는다.
3. 포트를 확인한다.
4. 그 후 HTTP 요청 메시지를 생성한다.
       HTTP 요청 메시지 형식
       GET /search?q=yongju&hl=ko HTTP/1.1Host:www.google.com

5. Socket 라이브러리를 통해 TCP/IP 계층에 데이터를 전달한다. 전달할 때 3 Way HandShacke를 통해 서버와 연결을 확인한 후 TCP/IP 계층에 데이터를 전달한다.
6. TCP/IP 계층에서 패킷을 생성하며 생성시 전달받은 HTTP 요청 메시지를 담는다.
7. 서버에서 패킷을 수신한다. 서버에서 수신할 때 TCP/IP 부분을 제외하고 전달된 HTTP 요청 메시지를 해석한다.
8. 서버에서 요청에 따른 HTTP 응답 메시지를 만들어 낸다.

        HTTP 응답 메시지 형식
        HTTP/1.1 200 OK
        Content-Type: text/html;charset=UTF-8 ( html 형식이며, 문자형식은 utf8이라는 정보 )
        Content-Length: 3423 ( 실제 html 데이터의 길이 )
        <html>
          <body>...</body>
        </html>

10. 서버에서 HTTP 응답 메시지를 웹브라우저에 전달한다.
11. 웹브라우저가 HTTP 응답 메시지를 수신하고 해석한 후 화면에 보여준다.
