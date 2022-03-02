# 2일에 공부한 내용을 적었습니다.

어제 다른 사람이 풀이한 코드를 옮겨보면서 공부하였습니다.<br><br>

```java
import java.util.function.BiFunction;

public enum Operator {
    PLUS("+", (first, second) -> first + second),
    MINUS("-", (first, second) -> first - second),
    MULTIPLY("*", (first, second) -> first * second),
    DEVIDE("/", (first, second) -> first / second);

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
}
```
먼저 Operator를 enum으로 설정합니다.<br><br>
자바에서 enum은 클래스와 같은 역할을 수행할 수 있습니다.<br><br>
멤버로 String인 symbol을 두고, 여기에 "+", "-", "\*", "/"을 저장합니다.<br><br>
두번째 멤버는 operation인데 자료형이 BiFunction입니다.<br><br>
BiFunctuni\<T, U, R\>인데 T와 U를 입력하여 R을 반환하는 구조입니다.<br><br>
여기서는 T, U, R 모두 Integer로 설정합니다.<br><br>
그리고 여기에 enum의 특성을 활용하면 연산자가 자신의 식을 가지도록 설정할 수 있습니다.<br><br>
operate라는 메소드를 생성하고 매개변수로 int형 first와 second를 받습니다.<br><br>
메소드 내부에서 멤버인 operation(자료형이 BiFunction)의 apply를 호출하고 first와 second를 매개변수로 전달합니다.<br><br>
멤버로 저장된 symbol이 "+"인 경우 enum상수 PLUS로 인식되어 apply식에서 first와 second를 더하도록 설정되었습니다.<br><br>
이제 Operator클래스의 객체가 operate연산을 수행하면 알아서 +연산을 해줍니다.<br><br>
-이면 -연산을 해주고 나머지 연산도 마찬가지입니다.<br><br>
이처럼 enum상수를 symbol과 식으로 묶어 놨기 때문에 해당 symbol에 걸맞는 식의 연산을 하도록 강제할 수 있습니다.<br><br>
이렇게 설정하면 더이상 switch를 할 필요없이 단순히 Operator객체의 operate연산만 수행하면 원하는 연산결과를 얻을 수 있습니다.<br><br>
이처럼 상태와 행위를 한 곳에서 관리하게 되는 예시는 **우아한형제들의 기술블로그에서 이동욱님이 작성한 Java Enum 활용기**에서도 확인할 수 있습니다.<br><br>
<a href="https://techblog.woowahan.com/2527/" target="_blank">Java Enum 활용기</a><br><br>
공부할 겸 거기서 사용한 코드 예시를 옮겨보려고 합니다.<br><br>

# 자바enum을 사용하면 첫번째 장점 : 데이터들간의 연관 관계를 표현
origin테이블에 있는 내용을 2개의 다른 테이블에 옮길 때 발생하는 문제를 예시로 들었습니다.<br><br>
origin테이블에서 "Y"/"N"으로 값을 표현하는데 이 값이 2개의 테이블에서는 각각 "true"/"false", "1"/"0"으로 표현되고 있습니다.<br><br>
이를 분류하기 위해 다음과 같은 코드가 필요합니다.<br><br>
```java
public class LegacyCase {
    public String toTable1Value(String originalValue) {
        if("Y".equals(originalValue)) {
            return "1";
        } else {
            return "0"
        }
    }
    
    public boolean toTable2Value(String originalValue) {
        if("Y".equals(originalValue)) {
            return true;
        } else {
            return false;
        }
    }
}
```
현재 이 코드는 현재 "Y", "1", true가 같은 값인지 알기 힘들고,<br><br>
if else의 코드 양이 많고 중복이 심합니다.<br><br>
이를 개선하기 위해 enum을 사용합니다.<br><br>
```java
public enum TableStatus {
    Y("1", true),
    N("0", false);
    
    private String table1Value;
    private boolean table2Value;
    
    TableStatus(String table1Value, boolean table2Value) {
        this.table1Value = table1Value;
        this.table2Value = table2Value;
    }
    
    public String getTable1Value() {
        return table1Value;
    }
    
    public boolean isTable2Value() { return table2Value; }
}
```
이렇게 enum을 활용하면 "Y"와 "1", true가 하나로 묶이게 됩니다.<br><br>
```java
@Test
public void origin테이블에서_조회한_데이터를_다른_2테이블에_등록한다() throws Exception {
    //given
    TableStatus origin = selectFromOriginTable();
    
    String table1Value = origin.getTable1Value();
    boolean table2Value = origin.getTable2Value();
    
    assertThat(origin, is(TableStatus.Y));
    assertThat(table1Value, is("1"));
    assertThat(table2Value, is(true));
}
```
이처럼 enum을 활용해 값을 묶어 놓으면 TableStatus의 객체 origin에서 바로 "1"과 true값을 구할 수 있습니다.

# 자바enum을 사용하면 두번째 장점 : 상태와 행위를 한 곳에서 관리
상황에 따라 다른 식을 적용해야 할 경우, 즉 위의 Operator처럼 그런 경우가 있습니다.<br><br>
여기서는 예시로 "CALC_A"인 경우 값 그대로, "CALC_B"인 경우에 10을 곱한 값을, "CALC_C"인 경우에는 3을 곱한 값을 계산하여 전달하는 경우입니다.<br><br>
가장 쉬운 방법은 static메소드를 작성하여 필요한 곳에서 그 메소드를 호출하는 방법입니다.<br><br>

```java
public class LegacyCalculator {
    public static long calculate(String code, long originalValue) {
        if("CALC_A".equals(code)) {
            return originalValue;
        } else if("CALC_B".equals(code)) {
            return originalValue * 10;
        } else if("CALC_C".equals(code)) {
            return originalValue * 3;
        } else {
            return 0;
        }
    }
}
```
이렇게 static메소드로 만들어서 사용하면 코드는 코드대로 조회하고, 계산은 별도의 클래스와 메소드를 이용해야 함을 알 수 있습니다.<br><br>
```java
@Test
publci void 코드에_따라_서로_다른_계산하기_기존방식 () throws Exception {
    String code = selectCode();
    long originValue = 10000L;
    int result = LegacyCalculator.calculate(code, originVlaue);
    
    assesrtThat(result, is(10000L));
}
```
이 상황의 문제점은 LegacyCalculator의 calculate메소드와 code가 서로 연관이 있음을 코드로 알 수 있는 방법이 없습니다.<br><br>
뽑아낸 코드에 따라 지정된 메소드만으로 계산되길 원하는데 지금은 현재 calculate에 문자열인자와 long타입으로 뭐든지 다 들어갈 수 있기 때문에<br><br>
히스토리를 잘 모르는 사람은 위와 같은 식을 사용할 경우 혼란이 생길 수 있습니다.<br><br>
그래서 이 code에 따라 특정하게 계산해야 하는 식을 enum을 통해서 묶을 수 있습니다.<br><br>

```java
public enum CalculatorType {
    CALC_A(value -> value),
    CALC_B(value -> value * 10),
    CALC_C(value -> value * 3),
    CALC_ETC(value -> 0L);
    
    private Function<Long, Long> expression;
    
    CalculatorType(Function<Long, Long> expression) { this.expression = expression; }
    
    public long calculate(long value) { return expression.apply(value); }
}
```

Function\<T, R\>의 경우 T를 function에 넣으면 R이 출력되는 구조입니다.<br><br>
이를 통해 각 코드마다 값이 그대로 나갈지 곱을 할지 곱을 한다면 얼마나 할지를 정할 수 있습니다.<br><br>
이 코드를 통해 테스트를 해보면<br><br>

```java
@Test
public void 코드에_따라_서로_다른_계산하기_Enum() throws Exception {
    CalculatorType code = selectType();
    long originalValue = 10000L;
    long reasutl = code.calculate(originalValue);
    
    assertThat(result, is(10000L));
}
```
이전에는 별도의 클래스의 정적메소드에 code와 originalValue값을 넣어 계산하였지만<br><br>
Enum을 활용한 후에는 code에게 직접 originalValue값에 대해 계산하도록 설정하였습니다.<br><br>

# 자바enum을 사용하면 세번째 장점 : 데이터 그룹 관리

결제라는 데이터는 결제 종류와 결제 수단 2개로 구분됩니다.<br><br>
결제 종류에는 크게 현금, 카드, 기타가 있고,<br><br>
현금이라는 결제 종류 밑에 결제 수단으로<br><br>
계좌이체, 무통장입금, 현장결제, 토스 등이 있고,<br><br>
카드라는 결제 종류 밑에 결제 수단으로<br><br>
페이코, 신용카드, 카카오페이, 배민페이 등이 있습니다.<br><br>
그리고 기타라는 결제 종류 밑에 결제 수단으로<br><br>
포인트와 쿠폰 등이 있습니다.<br><br>
결제된 건이 어떤 결제수단으로 진행되었으며,<br><br>
해당 결제 방식이 어떤 결제 종류에 속하는지 확인하기 위해서 가장 간단한 방식은 다음과 같습니다.<br><br>

```java
public Class LegacyPayGroup {
    public static String getPayGroup(String payCode) {
        if("ACCOUNT_TRANSFER".equals(payCode) || "REMITTANCE".equals(payCode) || "ON_SITE_PAYMENT".equals(payCode) || "TOSS".eaquls(payCode)) {
            return "CASH";
        } else if("PAYCO".equals(payCode) || "CARD".equals(payCode) || "KAKAO_PAY".equals(payCode) || "BAEMIN_PAY".equals(payCode)) {
            return "CARD";
        } else if("POINT".equals(payCode) || "COUPON".equals(payCode)) {
            return "ETC";
        } else {
            return "EMPTY";
        }
    }
}
```

위의 코드만 봐서는 결제종류가 결제수단을 포함하고 있다는 것을 알기 어렵고 그냥 대체관계로 여겨집니다.<br><br>
입력값에 어떠한 문자열도 입력 받을 수 있기 때문에 결과값 역시 예측을 할 수 없습니다.<br><br>
결제종류가 추가될 경우 else if문이 증가됩니다.<br><br>
결제종류, 결제수단등의 관계를 명확히 표현하며,<br><br>
각 타입은 본인이 수행해야할 기능과 책임만 가질 수 있게 하기 위해 enum을 활용합니다.<br><br>

```java
public enum PayGroup {
    CASH("현금", Arrays.asList("ACCOUNT_TRANSFER", "REMITTANCE",  "ON_SITE_PAYMENT", "TOSS")),
    CARD("카드", Arrays.asList("PAYCO", "CARD", "KAKAO_PAY", "BAEMIN_PAY")),
    ETC("기타", Arrays.asList("POINT", "COUPON")),
    EMPTY("없음", Collections.EMPTY_LIST);
    
    private String title;
    private List<String> payList;
    
    PayGroup(String title, List<String> payList) {
        this.title = title;
        this.payList = payList;
    }
    
    public static PayGroup FindByPayCode(String code) {
        return Arrays.stream(PayGroup.values())
        .filter(payGroup -> payGroup.hasPayCode(code))
        .findAny()
        .orElse(EMPTY);
    }
    
    public boolean hasPayCode(String code) {
        return payList.stream()
        .anyMatch(pay -> pay.equals(code));
    }
    
    public String getTitle() { return title; }
}
```
이렇게 enum을 활용하여 enum 상수에 결제종류와 그에 해당하는 결제수단을 리스트로 묶었습니다.<br><br>
이제 각 enum상수들은 본인들이 가지고 있는 문자열리스트에서 code를 확인하여 자신에게 속하는지 확인할 수 있습니다.<br><br>
즉, 이를 확인하는 괸리 주체를 enum인 PayGropu에게 주었습니다.<br><br>
Arrays.stream에 매개변수로 enum클래스인 PayGroup의 valuse를 전달함으로써 PayGroup의 enum상수 값들을 순회할 수 있습니다.<br><br>
이 stream에서 filter를 통해 원하는 값을 취하는데 filter에 거르기 원하는 식(hasPayCode)을 넣어 줍니다.<br><br>
hasPayCode는 enum객체가 가지고 있는 payList를 stream화하여서 list의 문자열 중에 매개변수로 입력 받은 code와 같은것이 있는지 여부(anyMatch)를 반환합니다.<br><br>
이 반환받은 값이 true이면 filter에서 거른 것 중에 찾은 게 있기 때문에 이 원소를 Optional로 리턴합니다.<br><br>
이 stream은 PayGroup의 상수들을 순회하고 있기 때문에 여기서 하나의 원소가 나올 것이기 때문에 이는 PayGroup의 객체일 것입니다.<br><br>
만약에 찾은 게 없다면 Optional이기 때문에 orElse를 통해 EMPTY를 반환합니다.<br><br>
이제 이 코드를 바탕으로 테스트를 해보면
```java
@Test
public void PayGroup에_직접_결제종류_물어보기 () throws exception {
    String payCode = selectPayCode();
    PayGroup payGroup = PayGroup.findByPayCode(payCode);
    
    assertThat(payGroup.getTitle(), is("카드"));
}
```
payGroup의 정적메소드에 payCode를 입력 받아 그에 맞는 PayGroup의 객체를 스스로 생성하고 있습니다.<br><br>
여기서 더 나아가 결제수단이 문자열이기 때문에 잘못된 문자열이 들어올 경우 관리가 안됩니다.<br><br>
이를 위해 결제수단 역시 enum으로 정할 수 있습니다.<br><br>

```java
public enum PayType {
    ACCOUNT_TRANSFER("계좌이체"),
    REMITTANCE("무통장입금"),
    ON_SITE_PAYMENT("현장결제"),
    TOSS("토스"),
    PAYCO("페이코"),
    CARD("신용카드"),
    KAKAO_PAY("카카오페이"),
    BAEMIN_PAY("배민페이"),
    POINT("포인트"),
    COUPON("쿠폰"),
    EMPTY("없음");
    
    private String title;
    
    PayType(String title) { this.title = title; }
    
    public String getTitle() { return title; }
```
이렇게 enum으로 결제종류를 만든 다음에 이를 반영하여 아까 구성한 enum을 수정하면

```java
public enum PayGroupAdvanced {
    CASH("현금", Arrays.asList(PayType.ACCOUNT_TRANSFER, PayType.REMITTANCE,  PayType.ON_SITE_PAYMENT, PayType.TOSS)),
    CARD("카드", Arrays.asList(PayType.PAYCO, PayType.CARD, PayType.KAKAO_PAY, PayType.BAEMIN_PAY)),
    ETC("기타", Arrays.asList(PayType.POINT, PayType.COUPON)),
    EMPTY("없음", Collections.EMPTY_LIST);
    
    private String title;
    private List<PayType> payList;
    
    PayGroupAdvanced(String title, List<PayType> payList) {
        this.title = title;
        this.payList = payList;
    }
    
    public static PayGroupAdvanced findByPayType(PayType, payType) {
        return Arrays.stream(PayGroupAdvanced.values())
        .filter(payGroup -> payGroup.hasPayCode(payType)
        .findAny()
        .orElse(EMPTY);
    }
    
    public static boolean hasPayCode(PayType payType) {
        return payList.stream()
        .anyMatch(pay -> pay == payType);
    }
    
    public String getTitle() { return title; }
}
```
이렇게 하면 문자열이 아니라 PayType으로 매개변수를 받기 때문에 타입 안정성까지 갖춰서 PayGroup과 관련된 처리를 할 수 있습니다.<br><br>

다시 이제 다음 코드로 넘어가도록 하겠습니다.<br><br>

```java
import java.util.regex.Pattern;

public class StringCalculator {
    private String formula;
    private int result = 0;
    private Operator currentOperator = Operator.PLUS;

    public void setFormula(String formula) {
        this.formula = formula;
    }

   public int calculateFormula() {
       for(String input : formula.split(" ")) {
            calculatePartial(input);
       }
       return result;
   }

   private void calculatePartial(String input) {
        String regExp = "^[0-9]*$";

        if(Pattern.matches(regExp, input)) {
            result = currentOperator.operate(result, Integer.parseInt(input));
            return;
        }

        for(Operator operator : Operator.values()) {
            if(operator.getSymbol().equals(input)) {
                currentOperator = operator;
                return;
            }
        }

       throw new IllegalArgumentException();
   }
}
```

정규식 표현을 쓰기 위해 regex.Pattern을 import해줍니다.<br><br>
StringCalculator의 멤버로 문자열인 formula, int형 result<br><br>
그리고 enum클래스인 operator를 두고 초기값은 PLUS상수로 설정합니다.<br><br>
setFormula에서 매개변수로 입력 받은 formula 문자열을 멤버로 저장합니다.<br><br>
calculateFormula에서 전체적인 계산을 하는데<br><br>
formula를 공백을 기준으로 substring하여 그 배열요소(문자열)을 calculatePartial에 매개변수로 전달합니다.<br><br>
calculatePartial에서는 문자열을 입력 받아 그 문자열이 정규식 표현으로 0~9에 해당하는 숫자인지 확인 후에<br><br>
숫자이면 멤버인 currentOperator의 operate함수의 매개변수로 전달하여 계산된 결과를 구합니다.<br><br>
숫자가 아니면 연산자일 것이기 때문에 어떤 연산지애 속하는지 알아내기 위해<br><br>
연산자의 상수들을 순회하면서 input과 알치하는지 확인합니다.<br><br>
일치한다면 input에는 숫자가 들어었기 때문에 currentOperator의 operate를 호출하여 계산합니다.<br><br>
그리고 calculatrPartial메소드를 종료시킵니다.<br><br>
일치하지 않는다면 input에는 숫자가 저장되어 있지 않기 때문에<br><br>
Operator의 상수들을 순회하면서 input과 같은 연산자가 있는지 확인합니다.<br><br>
같은 연산자가 있다면 currentOperator를 해당 operator로 바꿔서 저장해주고 메소드를 종료시킵니다.<br><br>
여기까지 메소드가 종료되지 않는다면 input에는 엉뚱한 문자열이 저장되어 있기 때문에<br><br>
IllegalArgumentException을 리턴합니다.<br><br>
결국에 calculateFormula에서 calculatePartial을 반복해서 호출하면 문자열 계산이 이뤄지고 이 결과를 반환합니다.<br><br>
