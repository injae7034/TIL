# int 배열을 List로 변환하기
<a href="https://hianna.tistory.com/552" target="_blank">int 배열을 List로 변환하기</a>를 참고하였습니다.<br><br>
배열을 List로 변환할 때, 원래는 주로 Arrays.asList()메소드를 사용하면 됩니다.<br><br>
하지만 배열의 원소가 int와 같은 primitive type인 경우 Array.asList()는 좀 다른 결과를 리턴합니다.<br><br>
## Arrays.asList()로 int 배열 변환하기 -> Fail
```java
import java.util.Arrays;
import java.util.List;

public class IntArrayConvertToList {
  public static void main(String[] args) {
    // int 배열
    int[] arr = { 1, 2, 3 };
    // Arrays.asList()
    List<int[]> intList = Arrays.asList(arr);
    // 결과 출력
    System.out.println(intList.size()); // 1
    System.out.println(intList.get(0)); // I@71bb301
    System.out.println(Arrays.toString(intList.get(0))); // [1, 2, 3]
  }
}
```
Arrays.asList()에서 int배열을 파라미터로 전달했을 때 나오기 기대했던 결과는 3개의 element를 가지고 있는<br><br>
List\<Integer\> 타입이지만 실제로는 1개의 element를 가지고 잇는 List\<int[]\>타입이 리턴되었습니다.<br><br>
Arrays.asList()메소드는 primitive 타입(int)을 Wrapper 클래스(Integer)로 형변환해주지 않기 때문에<br><br>
primitive 타입의 배열은 Arrays.asList()로는 List로 변환할 수 없습니다.<br><br>
int와 같은 primitive 타입의 배열을 List로 옮기기 위해서는 다른 방법을 이용해야 합니다.<br><br>
## 1. 반복문 사용하기
```java
import java.util.ArrayList;
import java.util.List;
public class IntArrayConvertToList {
  public static void main(String[] args) {
    // int 배열
    int[] arr = { 1, 2, 3 };
    // int -> List
    List<Integer> intList = new ArrayList<>();
    for (int element : arr) {
    intList.add(element);
    }
    // List 출력
    System.out.println(intList.size()); // 3
    System.out.println(intList); // [1, 2, 3]
  }
}
```

## 2. Stream 사용하기
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
public class IntArrayConvertToList {
  public static void main(String[] args) {
    // int 배열
    int[] arr = { 1, 2, 3 };
    // int -> List
    List<Integer> intList = Arrays.stream(arr)
            .boxed()
            .collect(Collectors.toList());
    // List 출력
    System.out.println(intList.size()); // 3
    System.out.println(intList); // [1, 2, 3]
  }
}
```

java 8 이후부터는 Stream을 사용할 수 있습니다.<br><br>
여기서 boxed() 메소드는 Primitive Stream 값들을 Wrapper Class로 바꿔줍니다.<br><br>
그 후 collect(Collectors.toList())를 이용하여 주어진 Stream을 List로 변경할 수 있습니다.<br><br>
double, long 등 다른 primitive 타입의 배열도 위와 같은 방법으로 List로 변환할 수 있습니다.<br><br>
