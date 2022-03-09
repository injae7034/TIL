# 9일에 공부한 내용을 적었습니다.

<a href="http://iloveulhj.github.io/posts/java/java-stream-api.html" target="_blank">Java 스트림 API</a>를 참고하였습니다.<br><br>
자바 스트림을 잘 사용하면 반복문을 대체할 수 있습니다.<br><br>

## 스트림과 컬렉션의 차이
1. 스트림은 요소들을 보관하지 않습니다. 요소들은 하부의 컬렉션에 보관되거나 필요할 때 생성됩니다.<br><br>
2. 스트림 연산은 원본을 변경하지 않습니다. 대신 결과를 담은 새로운 스트림을 반환합니다.<br><br>
3. 스트림 연산은 가능하면 지연(lazy)처리 됩니다. 즉, 결과가 필요하기 전에는 실행되지 않음을 의미합니다.<br><br>

## 스트림 API의 3단계
스트림 생성().중개 연산().최종 연산()<br><br>
1. 스트림을 생성합니다.<br><br>
2. 초기 스트림을 다른 스트림으로 변환하는 중간 연산들을 하나 이상의 단계로 지정합니다.<br><br>
3. 결과를 산출하기 위해 최종 연산을 적용합니다. 이 연산은 앞선 지연 연산들의 실행을 강제하고 하고 이후로는 해당 스트림은 더이상 사용할 수 없습니다.<br><br>

## 스트림 생성자
Collections.streams(), Collections,parallelStream()<br><br>
Arrays.streams(\*)<br><br>
IntStream.range(...), IntStream.rangeClosed(...)<br><br>
LongStream.range(...), LongStream.rangeClosed(...)<br><br>
Stream.of(\*), IntStream.of(\*), LongStream.of(\*), DoubleStream.of(\*)<br><br>
등등<br><br>

## 중개 연산자
<ul>
  <li>스트림을 받아서 스트림을 반환합니다</li>
  <li>무상태 연산과 내부 유지 연산으로 나누어집니다.</li>
  <li>기본적으로 지연(lzay) 연산 처리(성능 최적화)를 합니다.</li>
</ul>

## 최종 연산자
<ul>
  <li>스트림의 요소들을 연산 후 결과 값을 반환합니다.</li>
  <li>최종 연산 시 모든 연산을 수행합니다.(반복 작업 최소화)</li>
  <li>이후 더 이상 스트림을 사용할 수 없습니다.</li>
</ul>

## 스트림 생성
아래는 컬렉션을 스트림으로 변환하는 예제입니다.<br><br>
```java
//요소가 있는 스트림
Stream<String> stream = Stream.of(contenrs.spilit("[\\P{L}]+"));
Stream<String> stream = Stream.of("Using", "Stream", "API", "From", "Java8");
String[] wordArray = {"Using", "Stream", "API", "From", "Java8"};
Stream<String> stream = Arrays.stream(wordArray, 0, 4);
//요소가 없는 빈 스트림
Stream<String> stream = Stream.empty();
```

## filter, map, flatMap 메소드
### stream.filter()
filter는 특정 조건과 일치하는 모든 요소를 담은 새로운 스트림을 반환합니다.<br><br>
filter의 인자는 Predicate\<T\>, 즉, T를 받고 boolean을 반환합니다.<br><br>
```java
Stream<String> longStream = stream.filter(w -> w.length() > 5);
```
### stream.map()
스트림에 있는 값들을 특정 방식으로 변환하여 새로운 스트림으로 반환합니다.<br><br>
```java
stream<Character> mapStream = stream.map(s -> s.charAt(0));
```

### stream.flatMap()
스트림을 하나의 스트림으로 합쳐서 하나의 새로운 스트림을 반환합니다.<br><br>
```java
Stream<Character> flatMapStream = stream.flatMap(w -> characterStream(w));

private Stream<Character> characterStream(String s) {
		List<Character> result = new ArrayList<>();
		for (char c : s.toCharArray()) {
			result.add(c);
		}
		return result.stream();
	}
```
