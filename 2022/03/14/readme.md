# 14일에 공부한 내용을 적었습니다.
<a href="https://codechacha.com/ko/java8-supplier-example/" target="_blank">Java 8 - Supplier 예제</a>를 참고하였습니다.<br><br>
Supplier는 인자를 받지 않고 Type T 객체를 리턴하는 함수형 인터페이스입니다.<br><br>
```java
public interface Supplier<T> {
    T get();
}
```
## Example 1 : 단순 문자열 반환
```java
import java.util.function.Supplier;

public class SupplierExample1 {

    public static void main(String[] args) {

        Supplier<String> supplier = ()-> "HelloWorld";
        System.out.println(supplier.get());
    }
}
```
"HelloWorld"가 출력됩니다.<br><br>

## Example 2 : 객체 생성 및 반환
```java
import java.util.function.Supplier;

public class SupplierExample2 {

    public static void main(String[] args) {

        Supplier<Item> supplier = ()-> new Item(10, "Hello");

        Item result = supplier.get();

        System.out.println(result.getId() + ", " + result.getValue());
    }
}

public class Item {
    private int id;
    private String value;
    public Item(int id, String value) {
        this.id = id;
        this.value = value;
    }

    public int getId() {
        return id;
    }

    public String getValue() {
        return value;
    }
}
```
10과 Hello가 출력됩니다.<br><br>

## Example 3 : 메소드 참조방법
```java
import java.util.function.Supplier;

public class SupplierExample3 {

    public static void main(String[] args) {

        Supplier<String> supplier= SupplierExample3::getHelloWorld;
        System.out.println(supplier.get());
    }

    public static String getHelloWorld() {
        return "Hello World";
    }
}
```

## Example 4 : 원시자료형 값 반환
```java
import java.util.function.*;

public class SupplierExample4 {

    static String hello = "hello";

    public static void main(String[] args) {

        BooleanSupplier booleanSupplier = () -> hello.equals("world");
        IntSupplier intSupplier = () -> hello.length();
        LongSupplier longSupplier = () -> hello.length();
        DoubleSupplier doubleSupplier = () -> 12.34 - hello.length();

        System.out.println(booleanSupplier.getAsBoolean());
        System.out.println(intSupplier.getAsInt());
        System.out.println(longSupplier.getAsLong());
        System.out.println(doubleSupplier.getAsDouble());
    }
}
```

## Example 5 : Stream.generate() 인자
Stream.genereate()는 Supplier를 인자로 받아 무한한 Stream을 생성합니다.<br><br>
예제이서는 Random 숫자를 리턴하는 Supplier를 정의하였습니다.<br><br>
limit(5)는 Stream을 5개로 제한하는 코드이고,<br><br>
forEach(System.out::println)는 loop를 돌면서 객체를 출력합니다.<br><br>
```java
import java.util.Random;
import java.util.function.*;
import java.util.stream.Stream;

public class SupplierExample5 {

    public static void main(String[] args) {

        Supplier<Integer> randomNumbersSupplier = () -> new Random().nextInt(100);

        Stream.generate(randomNumbersSupplier)
                .limit(5)
                .forEach(System.out::println);
    }
}
```
100이하의 난수 5개가 콘솔창에 출력됩니다.
