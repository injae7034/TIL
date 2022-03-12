# 12일에 공부한 내용을 적었습니다.
<a href="https://codechacha.com/ko/java8-predicate-example/" target="_blank">Java 8 - Predicate 예제</a>를 참고하였습니다.<br><br>
Predicate\<T\>는 Type T흫 인자로 받고 boolean을 반환하는 함수형 인터페이스입니다.<br><br>
## test()
```java
import java.util.function.Predicate

public class PredicateExample1 {

    public static void main(String[] args) {
        Predicate<Integer> predicate = (num) -> num > 10;
        boolean result = predicate.test(100);
    }
}
```
result는 true입니다.<br><br>

## and()
and()는 Predicate\<T\>들을 연결하는 chain 메소드입니다.<br><br>
Predicate\<T\>들의 결과들을 AND 연산하고 그 결과를 리턴합니다.<br><br>
and로 연결된 모든 predicate의 test가 true일 때 true가 반환됩니다.<br><br>
둘 중 하나라도 false이면 false가 리턴됩니다.<br><br>

```java
import java.util.function.Predicate;

public class PredicateExample2 {

    public static void main(String[] args) {
        Predicate<Integer> predicate1 = (num) -> num > 10;
        Predicate<Integer> predicate2 = (num) -> num < 20;
        boolean result = predicate1.and(predicate2).test(25);
        result = predicate1.and(predicate2).test(15);
    }
}
```
첫번째 result는 false, 두번째 result는 true입니다.<br><br>

## or()
or() 역시 and()처럼 Predicate들의 객체들을 연결하는 chain 메소드입니다.<br><br>
차이점은 or()은 Perdicate들의 OR연산에 대한 결과를 리턴합니다.<br><br>
즉, or()로 연결된 predicate 중에 하나만 참이어도 그 결과는 참을 리턴합니다.<br><br>
```java
import java.util.function.Predicate;

public class PredicateExample5 {

    public static void main(String[] args) {
        Predicate<Integer> predicate1 = (num) -> num > 10;
        Predicate<Integer> predicate2 = (num) -> num < 0;
        
        boolean result = predicate1.or(predicate2).test(5);
        result = predicate1.or(predicate2).test(15);
        result = predicate1.or(predicate2).test(-5);
    }
}
```
첫번째 result만 둘 다 false이기 때문에 false이고, 나머지 둘은 하나가 참이기 때문에 result에 true가 저장됩니다.<br><br>

## isEqual()
isEqual()은 Stream에서 사용될 수도 있으며, 인자로 전달된 객체와 같은지 확인합니다.<br><br>
```java
import java.util.function.Predicate;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class PredicateExample4 {

    public static void main(String[] args) {
        Stream<Integer> stream = IntStream.range(1, 10).boxed();
        
        stream.filter(Predicate.isEqual(5))
                .forEach(System.out::println);
    }
}
```
콘솔창에 5가 출력됩니다<br><br>

## negate()
negate()는 Predicate가 반환하는 boolean값과 반대되는 값을 리턴합니다.<br><br>
논리연산자 NOT이 앞에 붙었다고 생각하면 됩니다.<br><br>
```java
import java.util.function.Predicate;

public class PredicateExample6 {

    public static void main(String[] args) {
        Predicate<Integer> predicate = (num) -> num > 10;
        Predicate<Integer> negatePredicate = predicate.negate();
        
        boolean result = predicate.test(100);
        result = negatePredicate.test(100);
    }
}
```
첫번째 result는 true, 두번째 result는 첫번째 result의 반대값인 false가 저장됩니다.<br><br>

## Stream
Predicate는 Stream의 filter()메소드의 인자로 전달될 수 있습니다.<br><br>
```java
import java.util.function.Predicate;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class PredicateExample3 {

    public static void main(String[] args) {
        Stream<Integer> stream = IntStream.range(1, 10).boxed();
        
        stream.filter(nun -> num > 5)
                .forEach(System.out::println);
    }
}
```
결과는 5보다 큰 6, 7, 8, 9가 콘솔창에 출력됩니다.<br><br>
