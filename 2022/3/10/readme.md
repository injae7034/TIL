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
