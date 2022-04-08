# 8일에 공부한 내용을 적었습니다.

## 스프링5 프로그래밍입문 챕터7 AOP 프로그래밍 읽고 예제 코드 작성 및 실습

## 코드숨 3주차 과제 리뷰 개선
### 1. HelloController의 JavaDoc 주석 1줄로 축약함.
### 2. given은 사용하지 않고, MockMvc를 사용하여 TaskControllerMock_MVC_Test하기
MockMvc를 사용하면 반드시 given을 사용해야 하는 줄 알았는데 그게 아니었습니다.<br>
TaskService에 test 상황들을 추가하고, MockMvc를 컨트롤러로 활용하여 perform을 이용해 테스트하였고,<br>
CRUD의 모든 상황에 대해서 테스트를 통과하였습니다.<br>
다만 모든 Tasks를 읽는 항목 테스트에서 10개의 task를 TaskService에 넣었는데 이 10개가 다 잘들어갔는지 테스트하기 위해<br>10개 항목을 모두 반복문없이 검사하고 있는데 이를 반복문으로 할 방법을 아직 찾지 못했습니다.<br>
스트림을 사용하여 forEach를 하면 뭔가 한줄에 해결이 될거같기도 한데 아직은 해결하지 못해서 10줄 노가다를 하였습니다;;<br>
### 3.  Context에서 논리적으로 상황만들기
"클라이언트가 요청한 Task의 id가 Tasks에 없으면" 에서 tasks에 없는 상황을 코드를 통해서 컨텍스트 안에 표현해보기<br>
1. Context의 setUp에서 비어있는 taskService에 10개의 task를 채워넣음.<br>
2. 그러므로 현재 tasksSize는 10개임.
3. 11번째 id를 조회함.(detail)
4. size가 10개인데 size보다 큰 id를 조회하기 때문에 TaskNotFoundException이 발생함.
