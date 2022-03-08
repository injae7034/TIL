# 8일에 공부한 내용을 적었습니다.
# 문자열계산기
자바 플레이그라운드 with TDD, 클린코드에서 문자열계산기를 구현하였는데<br><br>
테스트나 객체 지향적으로 미숙한 풀이여서 다른 사람의 풀이를 보고<br><br>
그것을 참고하여 공부하였습니다.<br><br>
이 후 다른 사람이 했던 풀이를 다시 보지 않고,<br><br>
복기하면서 그 코드를 구현하였습니다.<br><br>
남의 코드를 보면서 공부할 때는 다 이해했고, 쉽게 따라 할 수 있을 거 같았는데<br><br>
막상 안보고 그대로 구현해보려고 하니 잘 안되어서 시간이 좀 걸렸습니다.<br><br>
먼저 main 패키지에는 production code가 있고<br><br>
test 패키지에는 이를 test하는 code가 있도록 설정하였습니다.<br><br>
최대한 TDD로 구현해보기 위해 먼저 testCode 작성 후에 productionCode를 작성하는 식으로 하였습니다.<br><br>
## Formula test code
```java
package Calculator;

import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;

public class FormulaTest {

    @Test
    void FormulaConstructorTest() {
        Formula formula = new Formula("2 + 3 * 4 / 2");
        assertThat(formula.getFormula()).isEqualTo("2 + 3 * 4 / 2".split(" "));
    }
}
```
## Formula production code
```java
package Calculator;

public class Formula {
    private String[] formula;

    public Formula(String formula) {
        this.formula = formula.split(" ");
    }

    public String[] getFormula() {
        return this.formula;
    }
}
```
## Operator test code
```java
package Calculator;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;
import static org.assertj.core.api.Assertions.assertThatIllegalArgumentException;

public class OperatorTest {

    @Test
    void plusTest() {
        assertThat(Operator.isPlus().operate(6, "2")).isEqualTo(8);
    }

    @Test
    void minusTest() {
        assertThat(Operator.isMinus().operate(6, "2")).isEqualTo(4);
    }

    @Test
    void multiplyTest() {
        assertThat(Operator.isMultiply().operate(6, "2")).isEqualTo(12);
    }

    @Test
    void devideTest() {
        assertThat(Operator.isDevide().operate(6, "2")).isEqualTo(3);
    }

    @Test
    @DisplayName("올바른 사칙연산을 입력했을 때 테스트")
    void findByOperatorTest() {
        Operator operator = Operator.findByOperator("+");
        assertThat(operator).isEqualTo(Operator.isPlus());
        operator = Operator.findByOperator("-");
        assertThat(operator).isEqualTo(Operator.isMinus());
        operator = Operator.findByOperator("*");
        assertThat(operator).isEqualTo(Operator.isMultiply());
        operator = Operator.findByOperator("/");
        assertThat(operator).isEqualTo(Operator.isDevide());
    }

    @Test
    void 사칙연산_이외의_문자를_입력했을_때_테스트() {
        assertThatIllegalArgumentException().isThrownBy(() -> {
            if(Operator.findByOperator("$") == null) {
                 throw new IllegalArgumentException();
            }
        });
    }
}
```
## Operator production code
```java
package Calculator;

import java.util.Collections;
import java.util.Map;
import java.util.function.Function;
import java.util.function.ToIntBiFunction;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public enum Operator {
    PLUS("+", (first, second) -> first + second),
    MINUS("-", (first, second) -> first - second),
    MULTIPLY("*", (first, second) -> first * second),
    DEVIDE("/", (first, second) -> first / second);

    private static final Map<String, Operator> operatorMap = Collections.unmodifiableMap(
            Stream.of(values()).collect(Collectors
                    .toMap(Operator::getOperator, Function.identity())));

    private String operator;
    private ToIntBiFunction<Integer, Integer> toIntBiFunction;

    Operator(String operator, ToIntBiFunction<Integer, Integer> toIntBiFunction) {
        this.operator = operator;
        this.toIntBiFunction = toIntBiFunction;
    }

    public int operate(int first, String second) {
        return toIntBiFunction.applyAsInt(first, Integer.valueOf(second));
    }

    public static Operator findByOperator(String key) {
        /*
        return Stream.of(values())
                .filter(operator -> operator.getOperator().equals(key))
                .findAny()
                .orElseThrow(() -> new IllegalArgumentException());
         */
        return operatorMap.get(key);
    }

    public String getOperator() {
        return operator;
    }

    public static Operator isPlus() {
        return Operator.PLUS;
    }

    public static Operator isMinus() {
        return Operator.MINUS;
    }

    public static Operator isMultiply() {
        return Operator.MULTIPLY;
    }

    public static Operator isDevide() {
        return Operator.DEVIDE;
    }
}
```
## Calculator test code
```java
package Calculator;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThatThrownBy;
import static org.assertj.core.api.AssertionsForClassTypes.assertThat;

public class CalculatorTest {
    private Calculator calculator;
    @BeforeEach
    void setUp() {
        calculator = new Calculator();
    }
    @Test
    void calculateTest() {
        int result = calculator.calculate(new String[]{"2", "+", "3", "*", "4", "/", "2"});
        assertThat(result).isEqualTo(10);
    }
    @Test
    void 공백문자_2개_이상_일_때_예외테스트() {
        assertThatThrownBy(() -> {
            calculator.calculate("2 + 3  * 4   / 2".split(" "));
        }).isInstanceOf(NumberFormatException.class);
    }
}
```
## Calculator production code
```java
package Calculator;

import java.util.regex.Pattern;

public class Calculator {
    Pattern numbers = Pattern.compile("^[0-9]*$");
    public int calculate(String[] formula) {
        Operator operator = Operator.PLUS;
        int result = 0;
        for(String value : formula) {
            if(numbers.matcher(value).find()) {
                result = operator.operate(result, value);
                continue;
            }
            operator = Operator.findByOperator(value);
        }

        return result;
    }
}
```
## Application
```java
package Calculator;

import java.util.Scanner;

public class Application {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("문자열 계산기");
        System.out.println("숫자와 사칙연산을 스페이스바를 이용하여 분리해서 입력하세요.");
        String formula = scanner.nextLine();
        Calculator calculator = new Calculator();
        int result = calculator.calculate(new Formula(formula).getFormula());
        System.out.println(result);
    }
}
```
