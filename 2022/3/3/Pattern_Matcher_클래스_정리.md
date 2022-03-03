# Pattern, Matcher Class 사용법과 메소드 정리

<a href="https://girawhale.tistory.com/77" target="_blank">Pattern, Matcher Class 사용법과 메소드 정리</a>를 참고하였습니다.<br><br>
자바에서 정규식을 활용해 문자열을 검증, 탐색을 돕는 Pattern과 Matcher클래스를 제공합니다.<br><br>
## Pattern
정규 표현식이 컴파일된 클래스, 정규 표현식 대상 문자열을 검증하거나 활용하기 위해 사용하는 클래스입니다.<br><br>
정규식(문자열)을 매개변수로 하는 complie메소드를 이용하여 Pattern클래스의 객체를 생성합니다.<br><br>
pattern 메소드를 호출하면 컴파일한 정규식(문자열)을 반환합니다.<br><br>
matcher메소드는 메소드로 정규식 패턴과 매칭할 문자열을 입력 받아 Mathcer클래스를 반환합니다.<br><br>
matches메소드는 정규식(문자열)과 매칭할 문자열을 입력 받아 둘이 매칭이 되면 true, 매칭되지 않으면 false를 반환합니다.<br><br>

## Matcher
Pattern클래스를 받아 대상 문자열과 패턴이 일치하는 부분을 찾거나 전체 일치 여부 등을 판별하기 위해 사용됩니다.<br><br>
pattern메소드를 호출하면 Matcher가 해석한 Pattern클래스의 객체를 반환합니다.<br><br>
group메소드를 호출하면 매치와 일치하는 문자열을 반환합니다.<br><br>
matches를 호출하면 패턴에 전체 문자열이 일치하는 경우 true를 반환합니다.<br><br>
find메서드를 호출하면 패턴이 일치하는 다음 문자열을 찾는데 다음 문자열이 있다면 true를 반환합니다.<br><br>

```java
import java.util.regex.Pattern;

public class StringCalculator {
    private static final Pattern regExp = Pattern.compile("^[0-9]*$");

   public int calculateFormula(String[] formulaArray) {

       int result = 0;
       Operator currentOperator = Operator.PLUS;

       for(String input : formulaArray) {
           if(regExp.matcher(input).find()) {
               result = currentOperator.operate(result, Integer.parseInt(input));
               continue;
           }
           currentOperator = Operator.findOperator(input);
       }

       return result;
   }
}
```
이 코드를 예시로 살펴보면 regExp는 Pattern의 complie메소드를 통해 생성된 Pattern클래스의 객체입니다.<br><br>
Pattern의 객체 regExp가 matcher메소드를 호출하면 정규식 패턴과 매개변수 input을 통해 Mathcer클래스를 생성합니다.<br><br>
여기서 find를 통해 다음 문자열이 있으면, 즉, 패턴과 문자열의 매칭이 일치하면 다음 문자열이 있기 때문에,<br><br>
숫자라는 말이고, 일치하지 않으면 숫자가 아니라는 말입니다.
