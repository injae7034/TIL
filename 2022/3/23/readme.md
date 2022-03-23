# 23일에 공부한 내용을 적었습니다.

# 어제 받았던 코드 리뷰를 반영하여 코드 수정함
## 1. App.java에 있던 8000을 LOCAL_HOST 상수로 변경함.
## 2.  ObjectMapper를 이용하지 않고 구현함.
그 과정에서 ObjectMapper사용하는 다른 메소드들은 쉽게 처리하였는데<br>
 toTask 메소드에서 String객체 body를 Task객체로 바꾸는 과정에서<br>
String객체가 한글일 경우 유니코드로 들어오는데 이를 그대로 입력하면 유니코드 그대로 출력이 되는 문제가 발생함.<br>
이를 한글로 변환하는 방법을 몰라서 방법을 찾느라 시간 소요를 많이 함.
## 3.  200, 201번이 중복되어서 여러 선택문에서 중복되는 코드가 발생함.
중복코드를 없애기 위해 int형 statusCode 변수를 생성하고 초기값을 200으로 설정함.<br>
POST에서만 statusCode값을 201로 변경해줌<br>
마지막에 httpExchange.sendResponseHeaders(statusCode, content.getBytes().length) 한줄만 작성하면 되도록 변경함.
## 4.  GET 메소드에서 현재 /tasks가 들어왔을 때만 전체 task를 출력하는 문제 발생
/tasks/가 들어와도 똑같은 결과가 나오도록 수정함.<br>
이를 위해 선택문에서 "|| path.equals("/tasks/") "을 추가함.
## 5. /tasks/a 가 들어오면 GET이나 DELETE에서  400 Bad Request가 발생함
이게 발생하는 이유는 Integer.valueof을 할 때 문자가 들어와서 NumberFormatException이 발생하기 때문임.<br>
이를 막기 위해 try catch문을 사용함.<br>
NumberFormatException이 발생하면 숫자를 입력하라는 안내 문구를 출력하도록 개선함.

## 기존에 작동이 안되었던 DELETE기능 돌아가도록 변경함
## 1. List의 remove메소드를 사용할 때 매개변수로 int형 대신 Integer를 넣으니까 작동하지 않았음.
이를 int형으로 바꿔서 매개변수로 넣어주니까 List의 remove메소드가 정상적으로 작동함.
## 2. DELETE한 후 Hello World!가 출력됨.
정상적으로 지워졌을 경우 content에 ""을 대입함으로써 Transfer-encoding: chunked 가 출력 되도록 수정함.
 
## 현재 과제에서 제시한 출력 결과가 나오도록 구현함

## 미흡한 점
하지만 아직 예외 처리가 완벽하지 않고, 코드 정리 또는 메소드 분리가 필요함.

<br><br>

# 오늘 올렸던 코드에서 미흡한 점 개선
## 1. 코드 정리 및 메소드 분리
### makeContent 메소드를 생성
매개변수로 String객체인 method와 String객체인 path를 받음<br>
method에 따라서 각각 정해진 메소드 호출하고 이 때, path를 매개변수로 전달함.
1. method가 GET이면 make_READ_content 메소드를 호출
2. method가 DELETE이면 make_DELETE_content 메소드를 호출
3. method가 POST이면 make_CREATE_content 메소드를 호출
4. method가 PUT 또는 PATCH이면 make_UPDATE_content 메소드를 호출
각 메소드 내부에서 원래 선택구조에서 처리하던 작업을 처리함으로써 메소드의 역할을 분리시킴.

## 2. BodyContent 클래스 생성
기존에 BufferedReader를 통해 받는 String객체가 유니코드인데 이를 바꿔줄 로직이 필요한데<br>
이를 BodyCotnent를 생성하면서 거기서 바꾸는 로직을 처리하도록 분리함.<br>
즉, BodyContent의 객체가 생성되면 BodyContent의 객체는 유니코드에서 한글로 바뀐 String객체를 가지고 있고<br>
이를 getter메소드를 통해 그냥 얻어오면 됨.<br>
BodyContent 클래스의 객체를 DemoHttpHandler의 멤버로 설정하여 makeContent를 할 때 이용할 수 있도록 함.

## 3. 예외처리
기존 로직에서 tasks에 없는 번호의 정보를 얻으려거나 지우려고 하거나 수정하려고 할 때  예외가 발생하면서 프로그램이 정상적으로 작동하지 않았는데 이에 대한 예외처리를 하여 적절한 안내문구를 출력하도록 수정함.

## 4. 미흡한 점
현재 작성하면서 BodyContent 에 미흡한 점이 발견되었는데 현재 로직에서 title을  한글로 입력하면 프로그램이 실행될 때 유니코드로 바뀌어서 들어오기 때문에 BodyContent를 생성할 때  한글로 바꿔서 저장함.<br>
하지만 title로 영어나 숫자를 입력하면 프로그램이 실행될 때 유니코드로 바뀌지 않기 때문에 BodyContent를 생성할 때 빈 컨텐츠가 되버림.<br>
오늘 시간이 되면 일단 이것을 먼저 수정해야 함.

<br><br>

# 오늘 올렸던 코드에서 미흡한 점 추가 개선
아까 올린 코드에서 BodyContent 에 미흡한 점을 어떻게 개선할 지 고민하다가 클래스보다는 DemoHttpHandler의 makeBodyContent 메소드를 가지는 것이 예외처리에 더 수월할 것 같다고 생각함.<br>
그래서 BodyContent 클래스를 제거하고, DemoHttpHandler의 makeBodyContent 메소드를 새로 정의함.<br>
그리고 DemoHttpHandler의 멤버로 BodyContent 클래스의 객체대신 String 객체로 변경함.

## 아쉬운 점
이로 인해 이제 title에 한글이나 영문이나 숫자가 들어와도 title에 저장하고 출력하는데 문제는 없지만<br>
현재 DemoHttpHandler에만 메소드가 너무 많이 과중된 느낌입니다.<br>
그리고 DemoHttpHandler가 가지고 있는 메소드가 과연 DemoHttpHandler가 가지고 있어야 하는 것이 맞는지 의문이 듭니다.<br>
따로 객체를 생성하여 메소드를 분리해서 가져갈 수 없는지 고민해봐야 할 거 같습니다.  
