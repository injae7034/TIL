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

## 기본 타입 스트림
스트림 라이브러리는 기본 타입 값들에 특화된 IntStream, LongStream, DoubleStream을 포함합니다.<br><br>
short, char, byte, boolean의 경우 IntStream을 이용합니다.<br><br>
float인 경우는 DoubleStream을 이용합니다.<br><br>
다음은 기본적인 정적 스트림 생성 예제입니다.<br><br>
```java
IntStream result = IntStream.of(1, 2, 3, 4, 5);
IntStream result = Arrays.stream(array, 0, 5);
IntStream result = IntStream.range(0, 5);//최대값 제외
IntStream result = IntStream.rangeClosed(0, 5);//최대값 포함
```
다음은 객체 스트림을 기본 타입 스트림으로 변환하는 예제입니다.<br><br>
```java
IntStream result = stream.mapToInt(String::length);
```
일반적으로 기본 타입 스트림을 대상으로 동작하는 메소드는 객체 스트림 대상 메소드와 유사합니다.<br><br>
다만 다음의 몇가지 차이점이 있습니다.<br><br>
1. toArray메소드는 기본 타입 배열을 리턴합니다.<br><br>
2. OptionalInt, OptionalLong, OptionalDouble을 리턴합니다.<br><br>
Optional 클래스와 유사하지만 get 메소드 대신 getAsInt, getAsLong, getAsDouble 등을 포함합니다.<br><br>
3. 객체 스트림에는 정의되어 있지 않은 합계, 평균, 최대값, 최소값을 리턴하는 sum, average, max, min 메소드를 포함합니다.<br><br>
4. summaryStatistic 메소드는 스트림의 합계, 평균, 최대값, 최소값을 동시에 보고할 수 있는 IntsummaryStatistic, LongsummaryStatistic, DoublesummaryStatistic타입을 반환합니다.<br><br>

## 함수형 인터페이스
대부분의 Stream의 API는 인자로 함수형 인터페이스를 받아서 처리합니다.<br><br>
이 때 함수형 인터페이스는 람다 표현식 또는 메소드 표현식으로 사용합니다.<br><br>
### 람다 표현식을 사용하지 않은 경우
```java
Stream<String> filterStream = stream.filter(new Predicate<String>() {
      @Override
      public boolean test(String s) {
              return s.length() >= 4;
      }
});
```
### 람다 표현식을 사용한 경우
```java
Stream<String> filterStream = stream.filter(s -> s.length() >= 4);
```

### Supplier\<T\>
매개변수는 없고, 반환 타입이 T입니다.<br><br>
즉, T 타입 값을 공급합니다.<br><br>
### Consumer\<T\>
매개변수가 T타입이고, 반환타입이 없는 void입니다.<br><br>
즉, T 타입 값을 소비합니다.<br><br>
### BiConsumer\<T, U\>
매개변수가 T와 U타입이고 반환타입이 없는 void입니다.<br><br>
즉, T와 U타입 값을 소비합니다.<br><br>
### Predicate\<T\>
매개변수가 T타입이고 반환형은 boolean입니다.<br><br>
즉, T타입을 입력 받아 boolean을 반환하는 함수입니다.<br><br>
### ToIntFunction\<T\>
매개변수가 T타입이고, 반환형은 int형으로 고정되어 있습니다.<br><br>
즉, T타입을 인자로 입력 받고, int 값을 반환하는 함수입니다.<br><br>
### ToLongFunction\<T\>
매개변수가 T타입이고, 반환형이 long형으로 고정되어 있습니다.<br><br>
즉, T타입을 인자로 입력 받고, long 값을 반환하는 함수입니다.<br><br>
### ToDoubleFunction\<T\>
매개변수가 T타입이고, 반환형이 double형으로 고정되어 있습니다.<br><br>
즉, T타입을 인자로 입력 받고, double 값을 반환하는 함수입니다.<br><br>
### IntFunction\<R\>
매개변수가 int형으로 고정되어 있고, R 타입을 반환합니다.<br><br>
즉, int를 인자로 입력 받아서 R타입을 반환하는 함수입니다.<br><br>
### LongFunction\<R\>
매개변수가 long형으로 고정되어 있고, R 타입을 반환합니다.<br><br>
즉, long을 인자로 입력 받아서 R타입을 반환하는 함수입니다.<br><br>
### DoubleFunction\<R\>
매개변수가 double형으로 고정되어 있고, R 타입을 반환합니다.<br><br>
즉, double을 인자로 입력 받아서 R 타입을 반환하는 함수입니다.<br><br>
### Function\<T, R\>
매개변수가 T 타입이고, 반환형이 R 타입입니다.<br><br>
즉, T 타입을 인자로 받고, R 타입을 반환하는 함수입니다.<br><br>
### BiFunction\<T, U, R\>
매개변수가 T와 U 타입이고, 반환형이 R 타입입니다.<br><br>
즉, T와 U 타입을 입력 받아서 R 타입을 반환하는 함수입니다.<br><br>
### UnaryOperator\<T\>
T타입을 매개변수로 입력 받아 T 타입을 반환하는 단항 연산자입니다.<br><br>
### BinaryOperator\<T\>
T타입 2개를 입력 받아 한 개의 T 타입을 반환하는 이향 연산자입니다.<br><br>
