# 30일에 공부한 내용을 적었습니다.
## HTTPie란 무엇인가?
HTTP curl을 대체할 http client 유틸리티 CLI(Command Line Interface) 도구입니다.<br>
간단한 데이터 조회 및 전송을 테스트해볼 수 있습니다.<br>
우리가 자주 사용하는 curl로도 테스트할 수 있지만 사용하기 불편한 점이 있고,<br>
또 Postman을 사용할 수도 있지만<br>
간단하게 테스트할 때는 HTTPie로 해보는 것도 좋을 것 같습니다.<br><br>
특징<br>
httpie 는 python 으로 개발된 콘솔용 http client 유틸리티로 curl 대신 http 개발 및 디버깅 용도로 사용 가능하며 다음과 같은 특징이 있다.<br>
1. curl 에 비해 사용이 쉬움
2. json 지원 기능 내장
3. 출력을 포맷팅하여 보여주므로 가독성이 뛰어남
4. Form 과 file 업로드가 쉬움
5. HTTP 인증 및 커스텀 헤더 설정등
<br>

## CURL 이란 무엇인가?
**cURL = Client URL**<br>
클라이언트에서 커맨드 라인이나 소스코드로 손 쉽게 웹 브라우저 처럼 활동할 수 있도록 해주는 기술(커맨드라인 Tool 혹은 라이브러리)<br>
서버와 통신할 수 있는 커맨드 명령어 툴이다.<br>
웹개발에 매우 많이 사용되고 있는 무료 오픈소스이다.<br>
curl의 특징으로는 다음과 같은 수 많은 프로토콜을 지원한다는 장점이 있다.<br>
다양한 지원 프로토콜들<br>
DICT, FILE, FTP, FTPS, Gopher, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, Telnet, TFTP<br>
또한 SSL 인증 방식 역시 가능하다.<br>
url을 가지고 할 수 있는 것들은 다할 수 있다.<br>
예를 들면, http 프로토콜을 이용해 웹 페이지의 소스를 가져온다거나 파일을 다운받을 수 있다.<br>
ftp 프로토콜을 이용해서는 파일을 받을 수 있을 뿐 아니라 올릴 수도 있다.<br>
심지어 SMTP 프로토콜을 이용하면 메일도 보낼 수 있다<br>


## HTTPie와 CURL 예시

curl
```curl
$ curl https://www.hahwul.com -X POST -d test=1234
```
httpie
```httpie
$ http -f POST https://www.hahwul.com test=1234
```
여기서 -f 는 form으로 form 포맷으로 전달하기 위한 옵션입니다.<br>
이렇게 보면 크게 차이가 안나지만 JSON 등의 포맷이 있는 경우 차이가 확연하게 나타나기 시작합니다.<br>
(httpie가 JSON이 기본값이기 때문에, 그게 아니여도 위에서 -f 옵션으로 form 모드로 사용하는것도 편리하죠)<br>

curl
```curl
$ curl -X PUT https://www.hahwul.com -d "{\"data\":\"test\"}"
```
httpie
```httpie
$ http PUT https://www.hahwul.com data=test
```
