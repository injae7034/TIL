# 3월 26일에 공부한 내용을 적었습니다.
## DRY원칙
Don't Repeat Yourself<br><br>
반복하지 마라<br><br>
```java
POST("POST", (resource, tasks) -> {
        String content = "";
        String bodyContent = resource.getContent();
        String path = resource.getPath();
        if(!bodyContent.equals("") && (path.equals("/tasks") || path.equals("/tasks/"))) {
            Task task = new Task(tasks.getSize() + 1L, bodyContent);
            tasks.add(task);

            content = JsonPrinter.printTask(tasks.getTask(tasks.getSize() - 1));
        }
```
 해당 enum HttpMethod에 상수 코드는 POST상수와 문자열 POST가 2번 쓰이기 때문에 DRY원칙을 어기는 코드입니다.<br>
 enum HttpMethod에 Task를 처리하는 로직이 들어오면 안됨<br>
 너무 편의성만 생각해서 코드를 짜면 위와 같은 불상사가 발생함.<br>
 코드 편의성도 좋지만 명확한 역할 정의와 그에 대한 역할 분담이 더 중요합니다.<br>
 
