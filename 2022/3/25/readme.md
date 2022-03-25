# 개선한 점
## 1. Tasks는 Tasks와 관련된 일만 하도록 변경함
기존 코드에서는 Tasks에서 json형태로 변경하는 것까지 하고 있었는데 <br>
어제 코드리뷰를 통해  변경할 필요성을 깨달음<br>
Task의 toString코드도 기존에는 json형태로 출력되도록 정의하였는데 이것도 개선함.<br>
### 1.1 Task의 toString코드
" "을 delimeter 상수로 설정하여 id와 title을 출력하도록 변경함
### 1.2 Tasks에 있는 JSON과 관련된 메소드를 모두 제거함.
### 1.3 Tasks에서 getTask(int index)메소드 새로 정의
index를 통해 Task를 구할 수 있도록 정의함.
### 1.4 Tasks 메소드 IndexOutOfBoundsException예외처리
set, remove, getTask 모두 index를 매개변수로 입력 받는데 이 때, index가 List\<Task\>의 index를 벗어나면<br>
IndexOutOfBoundsException예외가 발생하는데 이에 대한 예외처리를 해줌.<br><br>

## 2. Task를 JSON형태로 변경하기 위해 JsonPrinter 클래스 생성
JsonPrinter는 유틸리티클래스로 멤버는 하나도 없고, 2개의 정적 메소드를 가짐.<br>
### 2.1 printTask메소드
Task객체를 매개변수로 입력 받아서 하나의 Task만 JSON형태로 출력함.<br>
### 2.2 printAllTasks메소드
Tasks 객체를 매개변수로 입력 받아서 Tasks의 모든 Task들을 JSON에서 배열 형태로 출력함.

## 3. 예외처리
어제 "왜 throw를 던졌는데 예외처리가 안되지?"라는 바보같은 생각을 함;;<br>
당연히 throw만 던지고 그에 대한 처리(catch)를 안해주면 당연히 main이 마지막에 예외처리를 하기 때문에<br>
예외처리를 안한 것과 마찬가지인데 왜 혼자서 throw를 분명했는데 예외처리문구가 출력이 안되지라는 바보같은 의문을 가짐<br>
그래서 오늘은  **try catch**를 이용하여 **예외처리**해줌<br>

### 3.1 enum클래스인 StatusCode에 상수를 추가해줌.
StatusCode에 상수를 추가하지 않으면 **사용자가 잘못 입력하여 예외가 발생했는데도 200, OK가 뜨는 불상사가 발생**하기 때문<br>
그래서 BAD_REQUEST(400),  NOT_FOUND(404), METHOD_NOT_ALLOWED(405) 상수를 추가해줌.
### 3.2  enum클래스인 HttpMethod에 상수를 추가해줌.
현재 HttpMethod에는 4개의 상수(POST, GET, PUT, DELETE)가 있는데 사용자가 오타로 HTTP메소드를 잘못입력하거나<br>
정의하지 않은 다른 메소드를 입력할 경우에 대한 예외처리가 없었음.<br>
그래서 enum클래스인 HttpMethod에 WRONG_METHOD라는 상수를 추가해주고, operate 식으로는 안내 문구 문자열을 리턴함.<br>

### 3.3 Tasks 메소드에서 IndexOutOfBoundsException 예외처리
set, get, remove 메소드에서 IndexOutOfBoundsException이 발생할 수 있기 때문에 각각 다른 안내문구를 입력하여<br> 
예외가 발생하는 경우 IndexOutOfBoundsException예외를 throw함.<br>
set, get, remove 메소드는 enum클래스인 HttpMethod의 operate에서 호출하는데 여기서 예외처리를 하지 않고<br>
throws IndexOutOfBoundsException을 통해  operate 메소드를 호출한 DemoHttpHandler의 handle 메소드로 예외처리를 넘김<br>
DemoHttpHandler의 handle 메소드에서 try catch를 통해 IndexOutOfBoundsException이 발생한 경우<br>
IndexOutOfBoundsException의 객체에서 getMessage를 하여 content에 저장하고,<br>
StatusCode는 NOT_FOUND로 변경해줌.<br>
### 3.4 Resource 클래스의 makeBodyContent메소드에서 예외처리
POST를 할 때 body에 title을 입력하지 않고, body 내용을 기재하거나,<br>
title로 body 내용을 입력하고, 추가로 예를 들어 head등 다른 body 내용을 기재한 경우<br>
모드 IllegalArgumentException 예외를 발생시키고<br>
makeBodyContent를 호출한 DemoHttpHandler의 handle 메소드로 예외처리를 해줌.<br>
catch에서 IllegalArgumentException의 메세지를 구하고, statusCode는 BAD_REQUEST로 변경해줌.

### 3.5 title을 입력하고 =대신에 ==을 2번 입력할 경우 예외처리
이 때 Http메소드는 POST인데 body는 비어 있는 것으로 인식을 하여 예외처리를 안하면<br>
비어있는 상태로 Tasks에 추가한 다음 Create되었다는 오류가 발생함.<br>
이를 이해 DemoHttpHandler의 handle에서 statusCode가 POST인데 content가 비어있으면<br>
IllegalArgumentException 예외를 발생시키고 catch에서 IllegalArgumentException의 메세지를 구하고,<br>
statusCode는 BAD_REQUEST로 변경해줌.

### 3.6 사용자가 HTTP 메소드를 잘못 입력한 경우
이때는 HttpMethod의 정적 메소드인 findByHttpMethod에서 아무것도 찾지 못한 경우 WRONG_METHOD를 반환함<br>
WRONG_METHOD의 operate메소드를 호출하면 오류 안내 문구가 content에 반환될 것이고<br>
HttpMethod의 findByStatusCode를 통해 METHOD_NOT_ALLOWED(405)가 호출되면 이를 이용해<br>
화면에 출력하면 됩니다.

# 아쉬운 점
결국 Optional로는 예외처리를 해결하지 못함ㅠ<br>
예외처리를 많이 하다보니 코드에 try catch가 남발이 되어<br>
코드가 길어졌는데 이거 이렇게 하는 것이 맞나 싶은 의문이 듬;;
