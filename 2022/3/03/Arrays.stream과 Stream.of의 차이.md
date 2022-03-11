# Arrays.stream과 Stream.of의 차이
<a href="https://pjh3749.tistory.com/279" target="_blank">Arrays.stream과 Stream.of의 차이</a>를 참고하였습니다.<br><br>
Arrays.stream과 Stream.of은 스트림을 생성하기 위해 보편적으로 사용됩니다.<br><br>
non-primitive 타입, 즉, 원시 자료형이 아닐 경우에는 둘 다 Stream을 반환합니다.<br><br>
아래 코드의 Integer 배열일 경우 둘 다 Stream을 반환하기 때문에 결과가 같습니다.<br><br>
```java
Integer[] array = { 1, 2, 3, 4, 5 };
Arrays.stream(array).forEach(System.out::println);
Stream.of(arrray).forEach(System.out::println);
```
그러나 primitive 타입, 즉, 원시 자료형일 경우 차이가 발생합니다.<br><br>
예를 들어 원시자료형인 int배열의 경우 Arrays.stream()은 intStream을 반환하고<br><br>
Stream.of는 Stream<int[]>를 반환합니다.<br><br>
```java
int[] array = { 1, 2, 3, 4, 5 };
Arrays.stream(array).forEach(System.out::println);
Stream.of(arrray).forEach(System.out::println);
```
그렇기 때문에 이 경우 Stream.of을 사용하고 출력할 때는 flatMapToInt를 이용하여<br><br>
IntStream으로 변환시킨다음에 출력하여야 정상적으로 출력이 됩니다.<br><br>
Arrays.stream은 int, long, double 원시형 타입에 대해서는 작동을 하고<br><br>
그 이외에 다른 원시형 타입에 대해서는 작동하지 않는다는 차이점도 있습니다.<br><br>
