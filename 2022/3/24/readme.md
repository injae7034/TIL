# 24일에 공부한 내용의 핵심 키워드만 적었습니다.
<br>

# REST
REST는 Representational State Transfer라는 용어의 약자

# REST 구성
<ul>
  <li>자원(RESOURCE) - URI</li>
  <li>행위(Verb) - HTTP METHOD</li>
  <li>표현(Representations)</li>
</ul>

# REST의 특징
## 1. Uniform(유니폼 인터페이스)
## 2. Stateless (무상태성)
## 3. Cacheable (캐시 가능)
## 4. Self-descriptiveness (자체 표현 구조)
## 5. Client - Server 구조
## 6. 계층형 구조

# REST API 디자인 가이드
## 1. URI는 정보의 자원을 표현해야 한다.
URI에는 행위에 대한 표현이 들어가면 안됨.<br>
자원을 명시할 때는 동사보다는 명사를 사용해야함.<br>
## 2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.
### Create(POST)
### Read(GET)
### Update(PUT)
### Delete(DELETE)
### PATCH
PUT이 해당 자원의 전체를 교체하는 의미를 지니는 대신,<br>
PATCH는 일부를 변경한다는 의미를 지니기 때문에<br>
최근 update 이벤트에서 PUT보다 더 의미적으로 적합하다고 평가받고 있습니다.

## 3. 이외 규칙
### 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용
### URI 마지막 문자로 슬래시(/)를 포함하지 않는다.
### 하이픈(-)은 URI 가독성을 높이는데 사용
### 밑줄(\_)은 URI에 사용하지 않는다.
### URI 경로에는 소문자가 적합하다.
### 파일 확장자는 URI에 포함시키지 않는다.
## 4. 리소스간에 명확한 관계 표현을 위해 URI에 예외적으로 동사를 포함시킬 수 있음.
## 5. 자원을 표현하는 Collection과 Document
### Document는 하나의 객체
Document에 HTTP Method를 이용하면 하나의 객체만 영향을 미침.
### Collection은 객체들의 집합
Collection에 HTTP Method를 이용하면 Collection이 가지고 있는 전체 Document에 영향을 미침.
# HTTP 응답 상태 코드(HTTP status code)
HTTP Status code, 상태 코드는 HTTP 요청이 성공했는지 실패했는지를 서버에서 알려주는 코드
## 200 OK : 클라이언트의 요청을 정상적으로 수행함
상태 코드는 200으로 성공인데 body 내용엔 실패에 관한 내용을 리턴하면 안됨!
## 201 Created : 클라이언트가 어떠한 리소스 생성을 요청, 해당 리소스가 성공적으로 생성됨(POST를 통한 리소스 생성 작업 시)
HTTP 헤더의 Content-Location를 이용하여 만들어진 리소스 생성된 위치를 알려주면 더 좋음.
## 202 Accepted : 클라이언트의 요청은 정상적이나, 서버가 아직 요청을 완료하지 못함.
202 상태 코드에서 중요한 것은 작업의 확인이다.<br>
비동기 작업은 해당 요청이 언제 완료되는지 알 수 없다.<br>
클라이언트가 요청의 완료 여부를 확인할 수 있는 2가지 방법이 있다.<br>
1. Callback : 서버가 작업이 완료되면 클라이언트에게 알려주는 것
2. Polling : 클라이언트가 주기적으로 해당 작업의 상태를 조회하는 것
## 204 No Content : 클라이언트의 요청은 정상적이지만 컨텐츠를 제공하지 않음.
204의 경우 HTTP Response body가 아예 존재하지 않는 경우다.<br>
PUT으로 자원 수정 요청한 결과가 기존의 자원 내용과 동일하여 변경된 내용이 없을 때 204로 응답할 수 있음<br>
DELETE 삭제 요청으로 자원을 삭제하여 더 이상 존재하지 않고 그 자원을 참조하는 모든 자원도 삭제되어 더 이상 HTTP body를 응답하는 것이 무의미해졌을 때 사용.
## 400 Bad Request : 클라이언트의 요청이 부적절 할 경우 사용하는 응답 코드
400 상태 코드로 응답하는 것만으로는 부족함<br>
오류 발생 시 파라미터의 위치(path, query, body), 사용자 입력 값, 에러 이유를 꼭 명시하는 것이 좋음<br>
그 이유는 400 상태 코드는 서버에서 잘못됐다고 판단하는 응답이기 때문에<br>
서버는 그 요청이 왜 잘못되었는지 쉽게 알 수 있으나<br>
클라이언트는 알려주지 않으면 알기 어려움
## 401 Unauthorized : 클라이언트가 인증되지 않은 상태에서 보호된 리소스를 요청했을 때 사용하는 응답 코드
상태 코드 이름만 보면 권한(authorized)에 대한 내용처럼 보이지만, 사실 인증(authenticated)에 대한 이야기임<br>
401은 인증이 안돼 자원을 이용할 수 없는 상태고, 의미상 unauthenticated가 더 정확하다
## 403 Forbidden : 유저 인증상태와 관계 없이 응답하고 싶지 않은 리소스를 클라이언트가 요청했을 때 사용하는 응답 코드
403은 권한(authorized)에 대한 내용이다.<br>
401의 상태 코드명이 Unauthorized라 혼동의 여지가 있으나, 권한에 대한 내용이다.<br>
인증된 클라이언트가 권한이 없는 자원에 접근할 때 응답하는 상태 코드다
## 404 Not Found : 클라이언트가 요청한 자원이 존재하지 않음
REST API에선 크게 두 가지 경우에서 404 상태 코드를 응답한다.
1. 경로가 존재하지 않음 : API 프레임워크에선 경로(라우팅)에 대한 에러 처리를 자동으로 해줌.
2. 자원이 존재하지 않음 : 개발자가 존재하는 경로에 대한 요청이라도 자원이 존재하는지 파악 후, 존재하지 않는다면 404 상태 코드로 응답해야 한다.
## 405 Method Not Allowed : 클라이언트가 요청한 리소스에서는 사용 불가능한 Method를 이용했을 경우 사용하는 응답 코드
메소드란 POST, GET, PUT, DELTE 등 HTTP Method를 말한다.<br>
즉, 자원(URI)은 존재하지만 해당 자원이 지원하지 않는 메소드일 때 응답하는 상태 코드다.<br>
405 상태 코드는 OPTIONS 메소드와 HTTP header의 Allow와 연관되어있다.<br>
OPTIONS는 API가 허용하는 메소드가 어떤 것들이 있는지 확인하는 메소드다.<br>
405오류를 사전에 방지하기 위한 용도에 주로 쓰인다.<br>
이 때 응답 HTTP header의 Allow에 지원하는 메소드를 나열하여 응답한다.
## 409 Conflict : 클라이언트의 요청이 서버의 상태와 충돌이 발생한 경우
해당 요청의 처리 중 비지니스 로직상 불가능하거나 모순이 생긴 경우<br>
예를 들어 클라이언트의 삭제 요청은 받아들여져서 200 혹은 204로 응답해야 하지만,<br>
사용자의 게시물이 존재하는 경우 사용자를 삭제할 수 없다는 비지니스 로직이 있을 수 있다<br>
이렇게 API 사용에 있어 비지니스 로직상 모순이 발생하여 처리가 불가능한 경우 응답하는 상태 코드다.
## 429 Too Many Requests : 클라이언트가 일정 시간 동안 너무 많은 요청을 보낸 경우
비정상적인(DoS attack, Brute-force attack) 방법으로 자원을 요청하는 경우 응답한다.<br>
DoS는 가용성에 대한 공격이고 Brute-force는 기밀성에 대한 공격이다.<br>
하지만, 서버 입장에서 두 공격 모두 가용성에 피해를 입을 수 있다.<br>
서버가 감당하기 힘든 요청이 지속적으로 들어오면 서버는 해당 요청을 처리하기 위해 다른 작업을 처리하지 못할 수 있다.<br>
429 상태 코드는 이러한 경우 일정 시간 뒤 요청할 것을 나타내는 것이다.<br>
따라서 다음과 같이 HTTP hearder Retry-After을 이용한다.<br>
HTTP/1.1 429 Too Many Requests<br>
Retry-After: 3600<br>
클라이언트는 3600초 후에 다시 해당 자원에 대한 작업을 요청할 수 있다.
## 301 : 클라이언트가 요청한 리소스에 대한 URI가 변경 되었을 때 사용하는 응답 코드
## 5XX Server errors : 5XX 상태 코드들은 서버 오류로 인해 요청을 수행할 수 없다는 의미
클라이언트의 요청은 유효하여 작업을 진행했는데 도중에 오류가 발생한 경우다.<br>
API 서버의 응답에서 5XX오류가 발생해서는 안된다.<br>
보통 개발 과정에서 유효하지 않은 요청을 사전에 처리하지 않은 경우(400)에 많이 발생한다.<br>
클라이언트의 요청이 무조건 유효할 것이라 판단하고 개발하는 경우 오류를 발생시킬 수 있다.<br>
## 500 : 서버에 문제가 있을 경우 사용하는 응답 코드

# HTTP 와 REST
## HTTP(HyperText Transfer Protocol)는 웹 환경에서 정보를 주고받기 위한 프로토콜이다.
## REST(Representational State Transfer)는 분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍처이다.
HTTP는 웹 환경에서 정보를 송수신할 때 사용하는 약속이고, REST는 소프트웨어 아키텍처다.<br>
REST는 소프트웨어 아키텍처(설계 지침, 원리 등등)고 REST에서 클라이언트-서버 간 통신 시 HTTP를 사용한 것이다.<br>
REST는 HTTP를 사용하는 것이 일반적이자 사실상 필수적임.<br>
따라서 REST를 이용해 API를 설계하려면 HTTP에 대해서 알고 있어야 한다.<br>



# 참고한 공부 사이트
<a href="https://sanghaklee.tistory.com/57" target="_blank">RESTful API 설계 가이드</a><br>
<a href="https://meetup.toast.com/posts/92" target="_blank">REST API 제대로 알고 사용하기</a><br>
<a href="https://spoqa.github.io/2012/02/27/rest-introduction.html" target="_blank">REST 아키텍처를 훌륭하게 적용하기 위한 몇 가지 디자인 팁</a><br>
<a href="https://sanghaklee.tistory.com/61" target="_blank">REST API 관점에서 바라보는 HTTP 상태 코드(HTTP status code)</a><br>
