# 7일에 공부한 내용을 적었습니다.

# 스프링5 프로그래밍 입문 책 공부
챕터 4, 5, 6 읽고 예제 코드 작성 및 실습

# 코드숨 과제 코드리뷰 받은 후 개선사항
## 1. HelloController에 대한 JavaDoc 주석 달기
## 2. App클래스에서 getGreeting 메소드 제거 후 AppTest 클래스 제거
이후 테스트 코드를 실행시켰지만 아직 까지 별다른 문제를 찾지 못했음
## 3. Context 스코프의 역할
Context에서 Test에 사용할 id 변수 또는 title변수들을 미리 final로 선언해줌.
## 4. given을 사용하지 않고 TaskControllerMock_MVC_Test를 테스트할 방법 고찰
아직 어떻게 해야할지 모르겠음.<br>
자칫 잘못하면 TaskControllerTest와 중복이 될 거 같은데<br>
어떻게 해야 TaskControllerMock_MVC_Test와 TaskControllerTest를 구별해서 테스트할 수 있을지 아직 실마리를 찾지 못함.
