# 3일에 공부한 내용을 적었습니다.

# 다른 사람이 풀이한 문자열 계산기 코드를 보고 공부하였습니다.
```java
package study;

public class Formula {
    private String formula;

    public Formula(String formula) {
        this.formula = formula;
    }

    String[] splitFormula() {
        return formula.split(" ");
    }
}
```
기존에는 입력 받은 식을 분리하는 작업을 따로 Main이든 StringCalculator에서든 하고 있었는데<br><br>
이 분리하는 작업 역시 Formula클래스를 생성하여 거기에 역할을 분배하는 것이 좀 더 객체지향적이 코드인 것 같습니다.<br><br>
그래서 Formula클래스의 역할은 split하는 역할입니다.<br><br>

```java
package study;

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("문자열 계산기");
        System.out.println("공백으로 구분하여 원하시는 숫자와 연산자를 입력해주세요.");
        String value = scanner.nextLine();//사용자로부터 공백을 기준으로 문자열을 구분하여 입력 받음
        String[] formula = new Formula(value).splitFormula();
        StringCalculator stringCalculator = new StringCalculator();
        System.out.println(stringCalculator.calculateFormula(formula));
    }
}
```
이렇게 메인에서 사용자에게 문자열을 입력 받고 Formula클래스를 생성하면서 splitFormula연산을 실행하여<br><br>
공백으로 구분된 문자열배열을 formula에 반환 받은 다음에 이를 StringCalculator의 메소드인 calculateFormula에 넘겨 주면<br><br>
문자열이 계산되어 원하는 결과를 얻을 수 있습니다.<br><br>
StringCalulator의 코드를 보면<br><br>

```java
package study;

import java.util.regex.Pattern;

public class StringCalculator {

    private static final String regExp = "^[0-9]*$";
    private int result;
    private Operator currentOperator;

   public int calculateFormula(String[] formulaArray) {

       result = 0;
       currentOperator = Operator.PLUS;

       for(String input : formulaArray) {
           calculatePartial(input);
       }

       return result;
   }

   private void calculatePartial(String input) {

        if(Pattern.matches(regExp, input)) {
            result = currentOperator.operate(result, Integer.parseInt(input));
            return;
        }

        currentOperator = Operator.findOperator(input);

   }
}
```
문자열 상수로 0-9까지를 의미하는 정규식 표현을 정적멤버로 가집니다.<br><br>
계산 결과를 누적할 result를 int형 멤버로 가집니다.<br><br>
Enum클래스인 Operator의 객체인 currentOperator를 멤버로 가집니다.<br><br>
여기에 enum상수, 즉, 사칙연산 중 하나가 저장될 것입니다.<br><br>
메서드로는 main에서 호출한 calculateFormula가 있습니다.<br><br>
매개변수로는 문자열 배열입니다.<br><br>
메서드 내부에서 멤버인 result값을 0으로 초기화하고,<br><br>
currentOperator도 PLUS 상수로 초기화합니다.<br><br>
매개변수로 입력 받은 문자열을 for each 반복을 돌리면서 배열원소(문자열)를 하나씩 구합니다.<br><br>
반복문 내에서 하나씩 구한 원소를 calculatePartial메소드에 매개변수로 전달합니다.<br><br>
즉, 반복문 내에서 문자열배열의 첫 원소부터 마지막 원소까지 calculatePartial에 매개변수로 전달하며 메서드를 호출합니다.<br><br>
그리고 매번 메서드가 호출되고 나면 그 값을 result에 반환합니다.<br><br>
반복문이 끝나면 문자열 배열의 모든 원소에 대한 계산이 끝났기 때문에 result값을 반환하면 그 값이 문자열 계산이 끝난 값입니다.<br><br>
calculatePartil메소드는 calculateFormula에서만 호출이 될 것이기 때문에, 즉, 클래스 내부에서 호출이 되기 때문에 private으로 설정해줍니다.<br><br>
매개변수로 문자열을 받는데 이 문자열이 정규식 표현과 비교해 숫자가 맞는지 확인합니다.<br><br>
숫자가 맞다면 선택문에 들어가서 enum클래스의 객체인 currentOperator의 operate연산을 호출합니다.<br><br>
예를 들어 문자열배열의 첫 원소(숫자)가 들어갈 경우 현재 result값은 아까 초기화로 인해 0일 것이고,<br><br>
currentOperator도 아까 초기화로 인해 PLUS로 설정되어 있습니다.<br><br>
이 상태에서 operate연산을 실행하면 매개변수로 result(=0)과 첫번째 숫자(원소)문자열을 정수로 바꾼 다음 매개변수로 넣어 줍니다.<br><br>
예를 들어 숫자문자열이 8이 었으면 operate연산에서 현재 enum객체가 PLUS이기 때문에 0 + 8을 하여 result에는 8이 저장됩니다.<br><br>
그리고 메소드를 종료시키고 calculateFormula반복에서 다시 calculatePartial메소드가 호출되면<br><br>
이번에는 정상적이라면 연산자가 저장되어 있을 것이기 때문에 숫자 정규식 표현을 빠져나간 다음 Operator의 정적메소드인 findOperator메소드를 호출하고<br><br>
매개변수로 문자열을 전달하면, 그에 맞는 Operator 객체를 반환받아 이를 currentOperator에 저장합니다.<br><br>
이런식으로 누적하면서 result에 연산 중간과정의 값을 저장하고, currentOperator에 연산자를 갱신해주면서 문자열 계산을 하게 됩니다.<br><br>
다음으로는 Operator의 코드를 볼 차례입니다.<br><br>
```java
package study;

import java.util.Collections;
import java.util.Map;
import java.util.function.BiFunction;
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public enum Operator {
    PLUS("+", (first, second) -> first + second),
    MINUS("-", (first, second) -> first - second),
    MULTIPLY("*", (first, second) -> first * second),
    DEVIDE("/", (first, second) -> {
        if(second == 0) {
            throw new IllegalArgumentException();
        }
        return first / second;
    });

    private String symbol;
    private BiFunction<Integer, Integer, Integer> operation;

    Operator(String symbol, BiFunction<Integer, Integer, Integer> operation) {
        this.symbol = symbol;
        this.operation = operation;
    }

    public String getSymbol() {
        return this.symbol;
    }

    public int operate(int first, int second) {
        return operation.apply(first, second);
    }

    private static final Map<String, Operator> operatorMap = Collections.
            unmodifiableMap(Stream.of(values()).collect(Collectors
                    .toMap(operator -> operator.getSymbol(), Function.identity())));

    public static Operator findOperator(String symbol) {
        Operator operator = operatorMap.get(symbol);

        if(operator == null) {
            throw new IllegalArgumentException();
        }

        return operator;
    }

}
```
Operaotor는 Enum클래스로 상수로 PLUS, MINUS, MULTIPLY, DEVIDE를 가집니다.<br><br>
멤버로는 문자열인 symbol, 이것이 사칙연산 문자열을 나타냅니다.<br><br>
또한 멤버로 자료형이 BiFunction인 람다식을 멤버로 가집니다.<br><br>
Enum클래스의 각각의 상수에 symbol과 람다식을 같이 묶어줍니다.<br><br>
이렇게 되면 예를 들어, PLUS상수는 "+"라는 symbol과 두 매개변수를 더하는 연산식을 가지게 됩니다.<br><br>
여기서 DEVIDE의 경우만 0으로 나눌 때 예외처리를 해줍니다.<br><br>
생성자는 멤버가 2개이기 때문에 매개변수를 2개 가지는 생성자를 만들고, symbol을 구할 수 있게 getter메소드도 구현합니다.<br><br>
operate연산은 정수형 매개변수를 2개 받아서 이를 자신의 멤버인 람다식 operation의 apply에 전달해줍니다.<br><br>
이 apply가 호출되면 자신의 상수에 맞게 사칙연산을 수행하게 됩니다.<br><br>
그리고 추가적으로 멤버를 하나 더 설정하는데 이는 enum상수를 조회할 때마다 매번 stream을 열어서 이를 검색하면<br><br>
시간이 오래 걸리기 때문에 애초에 enum클래스가 생성될 때 한번만 stream을 생성하여 그 결과를 Map에 저장하고<br><br>
검색시 이 map을 이용하면 매번 stream을 여는 검색보다 시간이 훨씬 더 절약됩니다.<br><br>
이를 활용하기 위해 Map을 멤버로 두고 제한자는 멤버이기 때문에 당연히 private이고,<br><br>
enum객체가 없어도 사용하기 위해서 static으로 설정하고, 재할당을 금지하기 위해 final로 둡니다.<br><br>
그리고 초기값을 주는데 Collections의 정적메소드인 unmodifiableMap을 호출하여 Map의 객체를 반환받습니다.<br><br>
unmodifiableMap에 의해 생성된 Map의 객체는 수정이 불가합니다.<br><br>
unmodifiableMap의 매개변수로 enum상수들의 모든 값(values())을 stream화하여 collect메소드를 호출합니다.<br><br>
collect의 매개변수로 collectors의 toMap메소드를 호출하는데 첫번째 매개변수인자가 key값이고 두번째 매개변수 인자가 value값입니다.<br><br>
key값으로는 enum상수의 멤버인 symbol, 즉 사칙연산으로 정하고,<br><br>
value값은 현재 Function.identity()가 들어가 있는데 말 그대로 인자로 넣은 값 그대로라는 뜻입니다.<br><br>
현재 Map의 key값은 String이기 때문에 symbol과 일치하고, value값은 Operator로 설정하였기 때문에 value값은 Operator 객체가 됩니다.<br><br>
이렇게 Map을 생성하면 사칙연산이 key값이기 때문에 사칙연산으로 해당하는 value값인 Operator의 상수값을 바로 구할 수 있습니다.<br><br>
이를 findOperator메소드에 활용합니다.<br><br>
findOperator는 StringCalculator의 calculatePartial에서 호출되어야 하기 때문에 접근 제어자는 pulic이고<br><br>
사실상 findOperator를 통해 Opreator객체를 생성한다고 할 수 있기 때문에 Operator객체 유뮤와 별개로 호출할 수 있어야 해서 Static으로 설정합니다.<br><br>
반환값은 Opreator이고 매개변수는 문자열입니다.<br><br>
메소드 내부에서 매개변수로 입력 받은 문자열을 정적멤버인 Map에 key값으로 get하는 메소드를 호출합니다.<br><br>
매개변수로 입력 받은 문자열이 사칙연산 중에 하나면 유효한 값이 나올 것이고,<br><br>
사칙연산 중에 포함되지 않는 문자열이라면 null이 반환될 것입니다.<br><br>
null값이면 잘못된 문자열이 있다는 뜻이기 때문에 IllegalArgumentException을 throw하여 예외처리를 하고,<br><br>
사칙연산 중에 포함이 되어 있다면 그 사칙연산을 가지고 있는 enum클래스의 객체를 반환합니다.<br><br>
이제 이 코드들이 잘돌아가는지 확인하기 위한 테스트코드를 볼 차례입니다.<br><br>
```java
package study;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.*;

public class StringCalculatorTest {
    private StringCalculator stringCalculator = new StringCalculator();

    @DisplayName("정상 입력시 의도대로 계산된 값이 나오는지 테스트")
    @Test
    void calculateTest() {
        String[] formula = new Formula("5 + 5 * 5 / 25").splitFormula();
        int result = stringCalculator.calculateFormula(formula);
        assertThat(2).isEqualTo(result);
    }

    @DisplayName("0으로  나눌 시 IllegalArgumentException이 발생하는지 테스트")
    @Test
    void devideZeroTest() {
        assertThatIllegalArgumentException().isThrownBy(() -> {
            String[] formula = new Formula("2 + 2 * 10 / 0").splitFormula();
            stringCalculator.calculateFormula(formula);
        });
    }

    @DisplayName("입력 값이 null이거나 빈 공백 문자일 경우 IllegalArgumentException이 발생하는지 테스트")
    @Test
    void inputNullOrEmptySet() {
        assertThatIllegalArgumentException().isThrownBy(() -> {
            String[] formula = new Formula("2 +   * 2 / 2").splitFormula();
            stringCalculator.calculateFormula(formula);
        });
    }

    @DisplayName("사칙연산 기호가 아닌 경우 IllegalArgumentException throw")
    @Test
    void checkPermittedOperator() {
        assertThatIllegalArgumentException().isThrownBy(() -> {
            String[] formula = new Formula("2 $ 6 * 5 / 4").splitFormula();
            stringCalculator.calculateFormula(formula);
        });
    }

    @DisplayName("Operaotr plus 연산 테스트")
    @Test
    void operatorPlusTest() {
        assertThat(Operator.PLUS.operate(1, 5)).isEqualTo(6);
    }

    @DisplayName("Operator minus 연산 테스트")
    @Test
    void operatorMinusTest() {
        assertThat(Operator.MINUS.operate(9, 3)).isEqualTo(6);
    }

    @DisplayName("Opertor multiply 연산 테스트")
    @Test
    void operatorMultiplyTest() {
        assertThat(Operator.MULTIPLY.operate(2, 4)).isEqualTo(8);
    }

    @DisplayName("Operator divide 연산 테스트")
    @Test
    void operatorDevideTest() {
        assertThat(Operator.DEVIDE.operate(16 , 2)).isEqualTo(8);
    }
}
```
