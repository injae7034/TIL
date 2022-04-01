# List를 배열로 변환하기
<a href="https://hianna.tistory.com/551" target="_blank">List를 배열로 변환하기</a>를 참고하였습니다.<br><br>
## toArray() - java.util.List
```java
import java.util.ArrayList;
import java.util.Arrays;

public class ArrayListConversion {
public static void main(String[] args) {
    // ArrayList 준비
    ArrayList<String> arrList = new ArrayList<String>();
    arrList.add("A");
    arrList.add("B");
    arrList.add("C");
    // ArrayList를 배열로 변환
    int arrListSize = arrList.size();
    String arr[] = arrList.toArray(new String[arrListSize]);
    // 변환된 배열 출력
    System.out.println(Arrays.toString(arr));
  }
}
```

List를 배열로 변환하기 위해서 java.util.List의 toArray() 메소드를 사용하고<br><br>
파라미터로 변환할 때 배열의 타입과 같은 객체를 생성하여 넣어줍니다.<br><br>
위의 예제에서는 toArray()의 파라미터로 배열 객체를 만들어서 넘길 때<br><br>
정확한 배열의 길이를 지정하여 넘겨주었지만.<br><br>
아래와 같이, 배열의 길이를 '0'으로 해도 됩니다.<br><br>
```java
arrList.toArray(new String[0]);
arrList.toArray(new String[arrListSize]);
```
java.util.List의 toArray()메소드는 파라미터로 전달 박은 배열 객체의 길이가 원본리스트보다 작을 경우<br><br>
자동으로 원본 리스트의 size 크기로 배열을 만들어줍니다.<br><br>
반대로 원본 List의 길이보다 배열의 크기를 더 크게 지정하면 배열의 나머지 인덱트는 null로 채워집니다.<br><br>
