# 5. HTTP 메서드 활용(동적데이터 조회, HTML Form 데이터 전송, API전송 설계 예시)

<br/>

## API전송 설계 예시

<br/>

### POST 기반 등록 - 회원 관리 API를 개발 해야하는 경우

    회원 목록 /members -> GET
    회원 등록 /members -> POST
    회원 조회 /members/{id} -> GET
    회원 수정 /members/{id} -> PATCH, PUT, POST / PATCH는 수정할 데이터의 전부가 전달되지 않아도 부분만 변경가능하여 수정은 PATCH를 보통 사용한다. PUT은 정보를 덜 전달할 경우 덜 전달된 정보 자체가 대체 된다.
    회원 삭제 /members/{id} -> DELETE

- POST - 신규 자원 등록 특징
  - 컬렉션(Collection)
    - 서버가 관리하는 리소스 디렉토리를 의미한다.
      - 서버가 리소스의 URI를 생성하고 관리
        - 위 예시에서 컬렉션은 /members
      - 서버가 새로 등록된 리소스의 URI를 생성해준다.
        - HTTP/1.1 201 Created Location: /members/100
      - 클라이언트는 등록될 리소스의 URI를 모른다.
        - 회원 등록 /members -> URI는 /members이고 POST방식으로 단순히 전달

<br/>

### PUT 기반 등록 - 정적 컨텐츠 관리, 원격 파일 관리를 하는 경우

    파일 목록 /files -> GET
    파일 조회 /files/{filename} -> GET
    파일 등록 /files/{filename} -> PUT
    파일 삭제 /files/{filename} -> DELETE
    파일 대량 등록 /files -> POST

- PUT - 신규 자원 등록 특징
- 클라이언트가 리소스 URI를 알고 있어야 한다.(POST에서 /members만 넘긴것과는 차이가 있다.)
  - 파일 등록 /files/{filename} -> PUT
    - PUT /files/star.jpg
  - 클라이언트가 직접 리소스의 URI를 지정한다.
  - 스토어(Store)
    - 클라이언트가 관리하는 리소스 저장소를 의미한다.
    - 클라이언트가 리소스의 URI를 알고 관리해야 한다.
    - 위 예시에서 스토어는 /files(리소스파일경로)이다.

<br/>

### HTML FORM 사용(GET, POST만 지원됨)

    회원 목록    /members -> GET
    회원 등록 폼 /members/new -> GET
    회원 등록    /members/new , /members -> POST
    회원 조회    /members/{id} -> GET
    회원 수정 폼 /members/{id}/edit -> GET
    회원 수정    /members/{id}/edit , /members/{id} -> POST
    회원 삭제    /members/{id}/delete -> POST

- HTML FORM 사용 특징
  - GET, POST 방식만 지원된다.
    - 컨트롤 URI
      - 위 예시에서 POST 방식의 /new, /edit, /delete가 컨트롤 URI이다.
        - GET, POST만 지원아므로 제약이 있다.
        - 이런 제약을 해결하기 위해 동사로 된 리소스 경로 사용
        - HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API 포함)

<br/>

### 참고하면 좋은 URI 설계 개념

- 문서(document)
  - 일반 문서처럼 단일 개념일 때(파일 하나, 객체 인스턴스, 데이터베이스 row)
  - ex) /members/100, /files/start.jpg
- 컬렉션(collection)
  - 서버가 관리하는 리소스 디렉터리
  - 서버가 리소스의 URI를 생성하고 관리
  - ex) /members
- 스토어(store)
  - 클라이언트가 관리하는 자원 저장소
  - 클라이언트가 리소스의 URI를 알고 관리
  - ex) /files
- 컨트롤러(controller), 컨트롤 URI
  - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
  - 동사를 직접 사용
  - ex) /members/{id}/delete

<br/>

### 정리

- HTTP API - 컬렉션 스타일
  - POST 기반 등록
  - 서버가 리소스 URI 결정
- HTTP API - 스토어 스타일
  - PUT 기반 등록
  - 클라이언트가 리소스 URI 결정
- HTML FORM 방식
  - 순수 HTML + HTML form 사용
  - GET, POST만 지원
