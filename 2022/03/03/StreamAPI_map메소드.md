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

## 정수를 문자열로 변환(int -> String)
int 기본 타입을 주어진 문자열과 합쳐서 반환하는 메소드를 구현합니다.<br><br>
```java
public List<String> generateStrings(String prefix, int size) {
        return IntStream.range(0, size)
                .mapToObj(i ->  prefix + i)
                .collect(Collectors.toList());
}
```
IntStream의 정적메소드인 range를 호출하여 0부터 size를 제외한 size 전까지 숫자에 대해 IntStream객체를 생성합니다.<br><br>
이 IntStream객체, 즉, 기본형 특화 스트림을 일반 스트림으로 변환하기 위해 mapToObj메소드를 호출하고<br><br>
매개변수로 i를 prefix + i 로 바꿔주는, 즉, 정수를 문자열로 바꿔주는 Function 식을 넣습니다.<br><br>
그리고 이 결과를 List로 만들어 반환하도록 collect메소드를 호출합니다.<br><br>
잘 작동하는지 테스트를 합니다.<br><br>
```java
@Test
@DisplayName("정수를 문자열로 변환(int -> String)")
void generateStringsTest() {
        final var expected = List.of("wating-0", "waiting-1", "wating-2");
        final var result = generateStrings("wating-", 3);
        assertIterableEquals(expected, result);
}
```
테스트가 성공함을 알 수 있습니다.<br><br>

## 실외스포츠 이름 가져오기
```java        
public List<String> getOutdoorSportsName(List<Sport> sports, boolean isOutdoor) {
    return sports.stream()
        .filter(s -> s.isOutdoor() == isOutdoor)
        .map(Sport::GetName)
        .collect(Collectors.toList());
}
```
Sport클래스를 리스트로 가지고 있는 객체 sports의 stream메소드를 호출하여 Stream\<Sport\>객체를 생성합니다.<br><br>
filter메소드를 호출하여 List에 저장되어 있는 원소(Sport클래스의 객체)의 isOutdoor메소드의 결과값과<br><br>
매개변수로 입력 받은 isOutdoor의 boolean값이 서로 일치하는 것만 거릅니다.<br><br>
이렇게 걸러진 Stream객체를 map을 통해 원하는 자료로 만드는데, 걸러진 Sport객체들의 name을 구하기 위해<br><br>
getName메소드를 호출하고, 이들을 collect를 통하며 List\<String\>객체를 만들어 이 값을 반환합니다.<br><br>
제대로 작동하는지 테스트를 해봅니다.<br><br>
```java
@Test
@DisplayName("실외 스포츠 이름 가져오기(Sport -> String)"
void getOutdoorSportsNameTest() {
    private final List<Sport> sports = List.of(
        new Sports(1, "걷기", 40, false),
        new Sports(2, "스쿼시", 126, true),
        new Sports(3, "런닝머신", 110, true),
        new Sports(4, "등산", 84, false));
    final var expected = List.of("걷기", "등산");
    final var result = getOutdoorSportsName(sports, true);
    assertIterableEquals(expected, result);
}
```

## 스포츠들의 칼로리 평균 구하기
스포츠클래스의 리스트를 int형 리스트로 바꿉니다.<br><br>
```java
public OptionalDouble getAverage(List<Sport> sports) {
    return sports.stream()
        .mapToInt(Sport::burnedCarlories)
        .average();
}
```
List의 stream메소드를 호출하면 일반 스트림이 반환되는데<br><br>
mapToInt를 호출하여 이 일반스트림을 기본 자료형 특화인 IntStream으로 바꿔줍니다.<br><br>
IntStream으로 바꿀 때 각 원소가 현재 Sport의 객체들인데 그 객체들의 칼로리량으로 바꿔줍니다.<br><br>
그리고 IntStream의 average메소드를 통해 IntStream에 저장되어 있는 원소들의 평균을 구하여 반환해줍니다.<br><br>
이 때 반환형이 OptionalDouble인데 이는 기본 원시 자료형 특화 Optional입니다.<br><br>
average의 반환형이 OptionalDouble인 이유는 만약에 sports가 빈 리스트일 경우에 평균을 구하는 로직에서 Division by Zero가 발생할 수 있으므로<br><br>
이 경우에 결괏값이 없을 수 있다는 것을 명시적으로 알려주기 위해서 OptionalDouble 형태로 반환해줍니다.<br><br>
이제 이를 바탕으로 주어진 스포츠들의 평균 칼로리를 구합니다.<br>br>
```java
@Test
@DisplayName("스포츠들의 칼로리 평균구하기(Sport -> int)")
void getAverageTest() {
     private final List<Sport> sports = List.of(
        new Sports(1, "걷기", 40, false),
        new Sports(2, "스쿼시", 126, true),
        new Sports(3, "런닝머신", 110, true),
        new Sports(4, "등산", 84, false));
        
    final var expected = 90.0;
    final var result = getAverage(sports);
    assertTrue(result.isPresent());
    assertEquals(expected, result.getAsDouble());
}
```
getAverage의 반환형이 OptionalDouble이기 때문에 먼저 result가 비어있는지 여부를 assertTrue와 isPresent를 통해 확인합니다.<br><br>
이 후 getAsDouble을 통해 호출한 값과 expected값이 같은지 assertEquals를 통해 확인합니다.<br><br>

## 문자열을 문자열로 변환
```java
public List<String> getUpperStrings(List<String> strings) {
    return strings.stream()
            .map(String::toUpperCase)
            .collect(Collectors.toList());
}
```
문자열을 모두 대문자로 바꾸는 메소드인데 스트림의 특성상 getUpperStrings메소드가 실행되면 반환값은 대문자가 되지만<br><br>
매개변수의 값은 바뀌지 않습니다.<br><br>
이제 성공적으로 대문자로 바뀌는지 테스트합니다.<br><br>
```java
@Test
@DisplayName("문자열을 대문자로 변환")
void getUpperCaseTest() {
    final var strings = List.of("banana", "apple", "orange");
    final var expected = List.of("BANANA", "APPLE", "ORANGE");
    final var resutl = getUpperCase(strings);
    assertIterableEquals(expected, equals);
}
```

## 상품 카테고리에서 자동차를 분류한 후 자동차 클래스 생성하기 

```java
private final List<ProductEntity> productEntities = List.of(
        new ProductEntity(1L, "Aventador", "CAR", LocalDate.of(2018, 3, 28)),
        new ProductEntity(2L, "Ramen", "FOOD", LocalDate.of(2001, 4, 11)),
        new ProductEntity(3L, "Lego", "TOY", LocalDate.of(1980, 11, 5)),
        new ProductEntity(4L, "Veyron", "CAR", LocalDate.of(2009, 12, 9)),
        new ProductEntity(5L, "Modern Java in Action", "BOOK", LocalDate.of(2019, 8, 1)));

public List<Car> getCarsFromProductEntites(List<ProductEntity> productEntities) {
    return productEntities.stream()
        .filter(p -> "CAR".equals(p.getCategory()))
        .map(p -> new Car(p.getId(), p.getName(), p.getDate()))
        .collect(Collectors.toList());
}
```
ProductEntity클래스의 리스트 객체를 스트화시키고<br><br>
여기에 filter를 통해 ProductEntity의 category가 CAR와 일치하는 ProductEntity만 거릅니다.<br><br>
그 다음 map을 통해 ProductEntity의 객체에서 id와 name, date를 구해 car클래스를 생성하여<br><br>
이를 Car List로 만들어 반환합니다.<br><br>
이를 테스트 해봅니다.<br><br>
```java
@Test
@DisplayName("상품 카테고리에서 자동차를 분류한 후 자동차 클래스 생성하기")
void getCarsFromProductEntitesTest() {
    private final List<ProductEntity> productEntities = List.of(
        new ProductEntity(1L, "Aventador", "CAR", LocalDate.of(2018, 3, 28)),
        new ProductEntity(2L, "Ramen", "FOOD", LocalDate.of(2001, 4, 11)),
        new ProductEntity(3L, "Lego", "TOY", LocalDate.of(1980, 11, 5)),
        new ProductEntity(4L, "Veyron", "CAR", LocalDate.of(2009, 12, 9)),
        new ProductEntity(5L, "Modern Java in Action", "BOOK", LocalDate.of(2019, 8, 1)));
    final var expected = List.of(
                            new Car(1L, "Aventador", "CAR", LocalDate.of(2018, 3, 28)),
                            new Car(4L, "Veyron", "CAR", LocalDate.of(2009, 12, 9)));
    final var result = getCarsFromProductEntites(productEntities);
    assertIterableEquals(expected, result);
                            
}
```
