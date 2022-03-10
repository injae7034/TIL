# 10일에 공부한 내용을 적었습니다.
<a href="http://iloveulhj.github.io/posts/java/java-stream-api.html" target="_blank">Java 스트림 API</a>를 참고하였습니다.<br><br>
# 옵션 타입
Optinal 객체는 T 타입 객체 또는 객체가 없는 경우의 Wrapper 클래스입니다.<br><br>
## Optional\<T\>.get()
get메소드는 감싸고 있는 요소가 존재할 때는 요소를 반환하고 없을 경우는 NoSuchElementException을 throw합니다.<br><br>
## Optional\<T\>.isPresent()
isPresent메소드는 Optional 객체가 값을 포함하는지 알려줍니다.<br><br>
## Optional\<T\>.ifPresent()
옵션 값이 존재하면 해당 함수로 전달되며, 그렇지 않으면 아무 일도 발생하지 않습니다.<br><br>
## Optional\<T\>.map()
옵션 값이 존재하면 해당 함수를 호출한 후, Optional\<T\>를 리턴합니다.<br><br>
반환값에는 true, false, null을 가진 Optional을 가질 수 있습니다.<br><br>
## Optional\<T\>.orElse()
앞의 함수 실행여부와 상관없이 무조건 orElse구문을 실행합니다.<br><br>
다만 앞의 함수가 null이면 orElse의 결과과 선택받고, 앞의 함수가 null이 아니면 orElse의 실행결과는 버려집니다.<br><br>
## Optional\<T\>.orElseGet()
앞의 함수가 null이 아니면 orElseGet메소드는 실행되지 않고, 앞의 함수가 null이면 orElseGet메소드가 실행됩니다.<br><br>
## Optional\<T\>.orElseThrow()
앞의 함수가 null이 아니먄 orElseThrow는 실행되지 않고, 앞의 함수가 null이면 orElseThrow 메소드가 실행되면서 예외를 발생시킵니다.<br><br>
## Optional\<T\>.of()
Optional에 대상이 확실하게 있는 경우 사용합니다.<br><br>
## Optional\<T\>.ofNullable()
Optional에 대상이 있는지 불확실한 경우 사용하는데 null일 경우 Optional.empty()를, null이 아니면 Optional.of()를 반환합니다.<br><br>
# 결과 모으기
## 배열만들기
```java
String[] result = stream.toArray(String[]::new);
```
## Stream.collect()
병렬화를 지원하면서 한 객체의 스트림의 요소들을 모으려고 할 때 collect메소드를 사용하게 됩니다.<br><br>
collect메소드는 세 가지 인자를 받습니다.<br><br>
1. 공급자 : 대상 객체의 새로운 인스턴스를 만듭니다.<br><br>
2. 누산자 : 요소를 대상에 추가합니다.<br><br>
3. 결합자 : 두 객체를 하나로 병합합니다.<br><br>

### StringBuilder의 collect 메소드 예시
StringBuilder의 경우 카운트와 합계를 관리하는 객체이기 때문에 collect의 대상이 될 수 있습니다.<br><br>
```java
HashSet<String> result = stream.collect(HashSet::new, HashSet::add, HashSet::addAll);
```

### collect메소드에서 Collectors 이용
자바에는 세 함수를 제공하는 인터페이스를 가진 Collectors 클래스가 존재합니다.<br><br>
그래서 일일이 공급자, 누산자, 결합자를 지정할 필요 없이 간편하게 호출할 수 있습니다.<br><br>
```java
List<String> result = stream.collect(Collectors.toList());
Set<String> result = stream.collect(Collectors.toSet());
TreeSet<String> result = stream.collect(Collectors.toCollection(TressSet::new));
```
### collect 메소드에서 joinning
```java
String result = stream.collect(Collectors.joining());
String result = stream.collect(Collectors.joining("."));
```

### Stream.forEach(), Stream.forEachOrdered()
하나 하나의 값에 연산을 하는 방법도 있습니다.<br><br>
forEach의 경우에는 병렬스트림에서 순서를 보장할 수 없습니다.<br><br>
스트림 순서대로 조회하고 싶은 경우에는 forEachOrdered를 사용해야 합니다.<br><br>
하지만 이 경우에는 병렬성이 주는 대부분의 이점을 포기해야 합니다.<br><br>
두 메소드 모두 최종 연산이기 떄문에 스트림을 재사용할 수 없습니다.<br><br>
만약 재사용을 하고 싳다면 peek()메소드를 사용해야 합니다.<br><br>

