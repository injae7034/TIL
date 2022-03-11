# 1일에 공부한 내용을 적었습니다.
# 다른 사람의 문자열계산기 코드
```java
public class StringCalculator {
    private String[] operationType = {"+", "-", "*", "/"};

    public int calculate(String formula) {
        String[] input = formula.split(" ");
        int result = Integer.parseInt(input[0]);
        String operator = null;

        for(int i = 1; i < input.length; i++) {
            if(isPermittedOperator(input[i])) {
                operator = input[i];
            }
            else {
                result = partCalculate(operator, result, input[i]);
            }
        }

        return result;
    }

    private boolean isPermittedOperator(String input) {
        for(String operator : operationType) {
            if(input.equals(operator)) {
                return true;
            }
        }
        return false;
    }

    public int partCalculate(String operator, int result, String input) {
        if(input == null || input.equals("")) {
            throw new IllegalArgumentException();
        }
        switch (operator) {
            case "+":
                return plus(result, input);
            case "-":
                return minus(result, input);
            case "*":
                return multiply(result, input);
            case "/":
                return division(result, input);
            default:
                throw new IllegalArgumentException();
        }
    }

    public int plus(int result, String input) {
        return result + Integer.parseInt(input);
    }

    public int minus(int result, String input) {
        return result - Integer.parseInt(input);
    }

    public int multiply(int result, String input) {
        return result * Integer.parseInt(input);
    }

    public int division(int result, String input) {
        return result / Integer.parseInt(input);
    }
}
```
저와 비슷하게 구성되어있는데 차이점으로는<br><br>
저는 예외처리를 전혀하지 않았고<br><br>
switch를 사용하지 않고, if를 사용하였으며<br><br>
plus, minus와 같은 연산을 따로 만들지 않고<br><br>
if문에서 바로 연산처리를 하였습니다.

# 다른 사람의 문자열 계산기 테스트 코드
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.NullAndEmptySource;
import org.junit.jupiter.params.provider.ValueSource;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

public class StringCalculatorTest {
    private StringCalculator stringCalculator = new StringCalculator();

    @Test
    void plusTest() {
        assertEquals(5, stringCalculator.plus(2, "3"));
        assertThat(stringCalculator.plus(2, "3")).isEqualTo(5);
    }

    @Test
    void minusTest() {
        assertEquals(7, stringCalculator.minus(10, "3"));
        assertThat(stringCalculator.minus(10, "3")).isEqualTo(7);
    }

    @Test
    void multiplyTest() {
        assertEquals(10, stringCalculator.multiply(2, "5"));
        assertThat(stringCalculator.multiply(2, "5")).isEqualTo(10);
    }

    @Test
    void divisionTest() {
        assertEquals(10, stringCalculator.division(20, "2"));
        assertThat(stringCalculator.division(20, "2")).isEqualTo(10);
    }

    @Test
    void calculateTest() {
        assertEquals(10, stringCalculator.calculate("2 + 3 * 4 / 2"));
        assertThat(stringCalculator.calculate("2 + 3 * 4 / 2")).isEqualTo(10);
    }

    @ParameterizedTest
    @ValueSource(strings = {"#", "$", "&", "!", "@", "%", "^"})
    void partCalculatePermittedOperatorTest(String input) {
        assertThatIllegalArgumentException().isThrownBy(() -> {
            assertEquals(10, stringCalculator.partCalculate(input, 5, "5"));
            assertThat(stringCalculator.partCalculate(input, 5, "5")).isEqualTo(10);
        });
    }

    @ParameterizedTest
    @NullAndEmptySource
    void partCalculateNullInputTest(String input) {
        assertThatIllegalArgumentException().isThrownBy(() -> {
            assertEquals(10, stringCalculator.partCalculate("+", 5, input));
            assertThat(stringCalculator.partCalculate("+", 5, input)).isEqualTo(10);
        });
    }
}
```

StringCalculatorTest의 멤버로 StringCalculator객체를 설정하고 초기값으로 생성자를 이용해 바로 생성합니다.<br><br>
assertEquals을 이용해 기대값(5)와 실제값(stringCalculator의 plus(2, "3")연산의 반환값)이 같은지 테스트합니다.<br><br>
또는 assertThat을 이용하여 메소드 체이닝 형식으로 할 수 있는데,<br><br>
assertThat의 매개변수로 stringCalculator의 plus(2, "3")메소드를 넣고, 이 반환값이 5와 같은지<br><br>
isEqualTo를 이용해 확인합니다.<br><br>
이와 같은 방식으로 나머지 연산(minus, multiply, division)을 테스트합니다.<br><br>
stringCalculator의 calculate메소드를 테스트하는 방식도 동일합니다.<br><br>
이제 예외처리를 위한 테스트를 할 차례입니다.<br><br>
ParameterizedTest와 ValueSource를 통해 지정한 여러 매개변수를 테스트할 수 있도록 설정합니다.<br><br>
ValueSource에 있는 인자가 들어오면 IllegalArgumentException이 발생하기 때문에 이를 처리하기 위해<br><br>
assertThatIllegalArgumentException의 isThrownBy를 하여 그 내부에서 assertEquals의 매개변수로<br><br>
예외가 발생하는 상황인 매개변수 input을<br><br>
partCalculate의 매개변수로 전달합니다.<br><br>
그러면 예상한대로 IllegalArgumentException이 발생합니다.

# 이외에 배운 것

그 이외에도 일급컬렉션이 무엇인지, BiFunction을 사용해서 Enum에서 연산자와 메소드 처리를 같이 묶을 수 있는 방법과<br><br>
0-9까지 정규표현식과 Pattern을 이용한 match, Enum에서 find를 할 때 속도 향상을 위해 HashMap을 이용하는데 여기서,<br><br>
Function.identity를 이용하는 방법과 Stream Collect 사용 방법과 toMap과 Optional클래스는 무엇인지, 스트림API 사용방법,
Predicate, Consumer 등을 이용해서 람다식 표현(메소드에 매개변수로 식을 넘겨주는 방법) 등등<br><br>
모르는게 너무 많아서 블로그를 보면서 모르는게 나올 때마다 계속 찾아보니까 끝이 없네요...<br><br>
내일은 위에서 언급한 사항들을 정리해보는 시간을 가지려고 합니다.
