# 28일에 배운 내용입니다.
자바 플레이그라운드 with TDD, 클린코드에서 제시한 문자열 계산기를 요구한 사항대로 구현했습니다.<br><br>
<ul>요구사항
  <li>사용자가 입력한 문자열 값에 따라 사칙연산을 수행할 수 있는 계산기를 구현해야 한다.</li>
  <li>문자열 계산기는 사칙연산의 계산 우선순위가 아닌 입력 값에 따라 계산 순서가 결정된다. 즉, 수학에서는 곱셈, 나눗셈이 덧셈, 뺄셈 보다 먼저 계산해야 하지만 이를 무시한다.</li>
  <li>예를 들어 "2 + 3 * 4 / 2"와 같은 문자열을 입력할 경우 2 + 3 * 4 / 2 실행 결과인 10을 출력해야 한다.</li>
  <li>JUnit을 활용해 단위 테스트 코드를 추가해 구현한다.</li>
</ul>
여기에 else예약어를 사용하지 않고, 들여쓰기를 최대 1칸만 하도록 제한해서 코드를 구현했습니다.<br><br>
또한 calculate메소드 인수를 3개에서 1개로 줄이기 위해 sum과 operator를 멤버로 설정하였습니다.<br><br>

```java
import java.util.Scanner;

public class Main {
    private static int sum;
    private static String operator;
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("문자열 계산기");
        System.out.println("공백으로 구분하여 원하시는 숫자와 연산자를 입력해주세요.");
        String value = scanner.nextLine();//사용자로부터 공백을 기준으로 문자열을 구분하여 입력 받음
        String[] values = value.split(" ");//사용자로부터 입력받은 문자열을 공백을 기준으로 분리함

        sum = Integer.parseInt(values[0]);
        int i = 1;
        while(i < values.length)
        {
            operator = values[i];
            i++;
            calculate(values[i]);
            i++;
        }
        System.out.println(sum);
    }

    public static void calculate(String value)
    {
        if(operator.equals("+"))
        {
            sum += Integer.parseInt(value);
            return;
        }
        if(operator.equals("-"))
        {
            sum -= Integer.parseInt(value);
            return;
        }
        if(operator.equals("*"))
        {
            sum *= Integer.parseInt(value);
            return;
        }
        if(operator.equals("/"))
        {
            sum /= Integer.parseInt(value);
            return;
        }
    }
}
```
# 부족한 점
Junit을 어떻게 활용해 테스트할지 몰라서 일단 테스트는 못했습니다.<br><br>
그래도 테스트 빼고 나머지 요구사항들에서 대해서는 전부 충족하도록 설계하였습니다.<br><br>
그러나 다른 사람의 코드를 보니 사용자가 잘못 입력한 경우에 대한 예외처리를 전혀 하지 못했습니다.<br><br>
또한 사실상 Main클래스에서 멤버를 다 가지고 처리하고 있어서<br><br>
제대로 클래스를 생성하고 나눌 생각은 전혀 하지 못했습니다.
