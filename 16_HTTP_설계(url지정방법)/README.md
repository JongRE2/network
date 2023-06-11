# HTTP API 설계 예시

- POST 기반 등록
  - HTTP API - 컬렉션
  - 예) 회원 관리 API 제공 
- HTTP API - 스토어
  - PUT 기반 등록
  - 예) 정적 컨텐츠 관리, 원격 파일 관리 
- HTML FORM 사용
  - 웹 페이지 회원 관리 
  - GET, POST만 지원





## 1. 예시 : 회원 관리 시스템 

 : uri는 리소스(리소스는 미네랄을 캐다에서 미네랄을 의미한다.)로 식별하게 정의해야 한다!

### API 설계 - POST 기반 등록

'/members' 이런걸 컬렉션이라고 한다.

회원을 관리하는 uri에다가 {id}부분처럼 특정 데이터를 넣으면 회원이 새로 등록되게 한다는 등의 설계 내용을 client, 서버 개발자들끼리 약속을 한다.

- 회원 목록 /members **-> GET**
- 회원 등록 /members **-> POST**
- 회원 조회 /members/{id} **-> GET**
- 회원 수정 /members/{id} **-> PATCH(부분 수정), PUT, POST** [부분 수정일때는 보통 patch가 제일 적합함 / put이 의미가 있는 경우는, 게시글을 수정할때 처럼 완전히 내용을 덮을때 적합하다. / 이것도 저것도 다 애매하면 patch, put 다 아우르는 post를 쓰면 전부 해결 가능하다.]
- 회원 삭제 /members/{id} **-> DELETE**

 

### POST - 신규 자원 등록 특징

- 클라이언트는 등록될 리소스의 URI를 모른다.
  - 회원 등록 /members -> POST
  - POST /members

- <span style="color:yellow"><u>서버가 새로 등록된 리소스 URI를 생성해준다.</u></span>
  - HTTP/1.1 201 Created
  - Location: **/members/100**

- 컬렉션(Collection)
  - <u>서버가 관리하는 리소스 디렉토리</u> 를 말한다.
  - 서버가 리소스의 URI를 생성하고 관리 
  - 여기서 컬렉션은 /members



## 2. 예시 : 파일 관리 시스템

원격지에 파일을 관리하는 시스템이 있다고 가정하자.

### API 설계 - PUT 기반 등록

- **파일** 목록 /files **-> GET**
- *파일** 조회 /files/{filename} **-> GET** 
- **파일** 등록 /files/{filename} **-> PUT** 
- **파일** 삭제 /files/{filename} **-> DELETE** 
- **파일** 대량 등록 /files **-> POST**

( filename은 등록하려는 입장이 client이기때문에 client도 같이 공유되는 name이다. )



### 신규 자원 등록할때 POST와 구분되는 PUT의 특징

- <u>클라이언트가 리소스 URI를 알고 있어야 한다.</u>
  - 파일 등록 /files/{filename} -> PUT
  - PUT **/files/star.jpg**

- <span style="color:yellow"><u>즉 , 클라이언트가 직접 리소스의 URI를 지정한다.</u></span> 반면에 POST는 식별자 기본 리소스의 path로 끝난다. 파일의 내용까지 uri로 포함해서 넘기지 않는다.
- 스토어(Store)
  - 클라이언트가 관리하는 리소스 저장소 로 이해하자!
  - 클라이언트가 리소스의 URI를 알고 직접 관리한다. 그리고 서버는 오는대로 그냥 해주는 것이라고 보면 된다.
  - 여기서 스토어는 /files





## HTML FORM 사용

- HTML FORM은 **GET, POST**만 지원
- AJAX 같은 기술을 사용해서 API통신을 하면 GET, POST를 제외한 나머지 method 방식 통신도 해결 가능 -> 회원 API 참고 
- 하지만 여기서는 순수 HTML, HTML FORM만 가정하고 내용 설명한다. 이 경우에는 GET, POST만 지원하므로 제약이 있다.



### HTML FORM 설계 예시

**회원** 목록 - /members **-> GET** 

**회원** 등록 폼 - /members/new **-> GET** (회원정보 폼을 불러올때)

**회원** 등록 - /members/new, /members **-> POST** (입력후 저장(submit)버튼을 누를때 )

> 강사님은 `회원등록 폼`이랑  `회원 등록`의 uri를 똑같이 맞추는 것을 선호한다.
> 이유는 veridation이라는게 있는데, /member/new로 서버에 데이터를 던졌는데 서버에서 문제가 있어서 다시 POST의 최종결과를
> 회원등록 폼으로 다시 돌려보내야 하는 경우가 있다. 그럴때 경로를 2개 맞춰두면 기존의 POST경로를 안바꾸고 깔끔하게 해결이 될 수 있다.

**회원** 조회 - /members/{id} **-> GET** 

**회원** 수정 폼 - /members/{id}/edit **-> GET**  

**회원** 수정 - /members/{id}/edit, /members/{id} **-> POST**

**회원** 삭제 - /members/{id}/delete **-> POST** (uri 추가 식별하는 내용없이 메소드로 구분할 수 없으므로 <span style="color:yellow">컨트롤 uri(컨트롤러)</span>내용을 추가해줘야 한다.- 보통 이때 리소스 URI형태는 동사 형태다. )



###  <span style="color:yellow">컨트롤 URI</span>

: HTML FORM은 **GET, POST**만 지원하는 것을 보충해주는 개념

- GET, POST만 지원하므로 제약이 있어서 이런 제약을 해결하기 위해 <span style="color:yellow">**동사**</span>로 된 리소스 경로 사용 
- POST의 /new, /edit, /delete가 컨트롤 URI
- HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API 포함)



## ⭐️ 정리 - 참고하면 좋은 URI 설계 개념

(정답은 아니지만, 사람들이 많이 채택해서 사용하는 좋은 practies들이라고 생각하면 된다.)

- 문서(document)

  - 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)

  - 예) /members/100, /files/star.jpg 

- 컬렉션(collection)
  - 서버가 관리하는 리소스 디렉터리 
  - 서버가 리소스의 URI를 생성하고 관리 
  - 예) /members

- 스토어(store)
  - 클라이언트가 관리하는 자원 저장소 
  - 클라이언트가 리소스의 URI를 알고 관리 
  - 예) /files

- 컨트롤러(controller), 컨트롤 URI
  - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행 
  - 동사를 직접 사용
  - 예) /members/{id}/delete

> 참고 링크 : https://restfulapi.net/resource-naming/











