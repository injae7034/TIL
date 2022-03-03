# Stream API의 중간 연산 map 메소드

<a href="https://dev-kani.tistory.com/32" target="_blank">[Stream API] 중간 연산 - map 메서드</a>를 참고하였습니다.<br><br>

스트림은 파라미터로 제공되는 함수(Function\<T, R\>과 관련된 함수형 인터페이스)를 적용하여<br><br>
기존 요소를 새로운 요소로 매핑시키는 map이라는 메소드를 제공합니다.<br><br>
스트림은 기존의 있던 값을 변경시키지 않으므로 변경이라는 단어보다는<br><br>
기존의 값을 바탕으로 새로운 값을 생성한다는 의미로 map을 활용합니다.<br><br>
스트림객체가 있으면 그 객체는 그대로 두고, 그 객체를 바탕으로 map을 통해 이를 다른 형태로 변환을 시키는 것을 의미한다.<br><br>
map, mapToInt, mapToDouble, mapToLong, mapToObj가 존재합니다.<br><br>
IntStream, 즉, Int에 특화된 원시스트림의 경우 map, mapToDouble, mapToLong, mapToObj를 사용할 수 있고,<br><br>
DoubleStream은 map, mapToInt, mapToLong, mapToObj를 사용할 수 있고,<br><br>
LongStream은 map, mapToInt, mapToDouble, mapToObj를 사용할 수 있습니다.<br><br>
Stream은 map, mapToInt, mapToDouble, mapToLong을 사용할 수 있습니다.<br><br>

# map 메소드
map메소드를 살펴보면 일반 Stream과 기본형 특화 원시스트림(IntStream, DoubleStream, LongStream)에서 모두 사용할 수 있으며<br><br>
자기 자신의 자료형을 반환합니다.<br><br>
매개변수로는 IntStream의 경우 IntUnaryOperator,<br><br>
DoubleStream의 경우 DoubleUnaryOperator,<br><br>
LongStream의 경우 LongUnaryOperator<br><br>
Stream의 경우 원칙적으로 Stream\<R\>이기 때문에 매개변수는 Functuion\<T, R\>이고,<br><br>
T가 변환 전의 타입이고 R이 변환 후의 타입입니다.<br><br>
T와 R의 타입이 같을 수도 있고, 다를 수도 있습니다.<br><br>

# mapToInt 메소드
스트림을 IntStream으로 변환시켜주는 메소드입니다.<br><br>
IntStream을 제외한 모든 스트림에서 동일하게 제공됩니다.<br><br>
매개변수는 공통적으로 ToIntFunction\<T\>입니다.<br><br>
즉, 변환 후의 값인 R은 Int로 고정되어있고, 변경 전의 값인 T는 정해져 있지 않기 때문에<br><br>
T에는 여러 타입이 들어올 수 있습니다.<br><br>

# mapToLong 메소드
스트림을 LongStream으로 변환시켜주는 메소드입니다.<br><br>
LongStream을 제외한 모든 스트림에서 동일하게 제공됩니다.<br><br>
매개변수는 공통적으로 ToLongFunction\<T\>입니다.<br><br>
즉, 변환 후의 값인 R은 Long으로 고정되어있고, 변경 전의 값인 T는 정해져 있지 않기 때문에<br><br>
T에는 여러 타입이 들어올 수 있습니다.<br><br>

# mapToDouble 메소드
스트림을 DoubleStream으로 변환시켜주는 메소드입니다.<br><br>
DoubleStream을 제외한 모든 스트림에서 동일하게 제공됩니다.<br><br>
매개변수는 공통적으로 ToDoubleFunction\<T\>입니다.<br><br>
즉, 변환 후의 값인 R은 Double로 고정되어있고, 변경 전의 값인 T는 정해져 있지 않기 때문에<br><br>
T에는 여러 타입이 들어올 수 있습니다.<br><br>

# mapToObj 메소드
원시형, 즉 기본형 특화 스트림인 IntStream, LongStream, DoubleStream을 일반 Stream으로 바꿔주는 메소드입니다.<br><br>
기본형 특화 스트림의 mapToObj 메소드의 매개변수로는 변환 전의 값이 고정 되어 있고,<br><br>
변환 후에는 일반 Stream이기 때문에 값이 고정되어 있지 않아 R입니다.<br><br>
즉, IntStream의 경우 매개변수는 IntFunction\<R\>,<br><br>
LongStream의 경우 LongFunction\<R\>,<br><br>
DoubleStream의 경우 DoubleFunction\<R\>입니다.<br><br>

# 예제코드
## 문자열을 문자열 길이로 변환(String -> Integer)
```java
public List<Integer> getLength(List<String> strings) {
    return strings.stream()
        .map(String::length)
        .collect(Collectors.toList());
}
```
문자열 리스트를 매개변수로 받아서 stream메소드를 호출하여 일반 stream을 생성하고,<br><br>
map메소드를 호출하고 매개변수인 Function에 람다식 참조표현으로 String의 length메소드를 넣습니다.<br><br>
그럼 각 문자열 리스트의 각 원소들이 문자열의 길이, 즉, Integer값으로 바뀌고,<br><br>
이를 collect메소드를 호출하여 List로 만들어서 반환합니다.<br><br>
즉, 문자열의 길이가 저장된 Integer List가 반환됩니다.<br><br>
이를 테스트 해보면<br><br>
```java
@Test
@DisplayName("문자열을 문자열 길이로 변환(String -> Integer)")
void getLengthTest() {
    final var strings = List.of("banana", "apple", "orange");
    final var expected = List.of(6, 5, 6);
    final var result = getLength(strings);
    assertIterableEquals(expected, result);
}
```
**var**은 **타입추론 자료형**인데 **초기화값이 있는 지역변수**로만 선언이 가능합니다.<br><br>
strings의 경우 뒤에 문자열을 바탕으로 타입을 추론하면 List\<String\>임을 추론할 수 있습니다.<br><br>
expected의 경우도 List의 원소가 Int형이기 때문에 List\<Integer\>임을 추론할 수 있습니다.<br><br>
assertIterableEquals를 활용하여 배열과 Collection 자료형도 비교가 가능합니다.<br><br>
테스트 결과 성공적으로 문자열이 문자열 길이로 변환되었음을 알 수 있습니다.
