# 5일에 공부한 내용을 적었습니다.
자바 플레이그라운드 with TDD, 클린코드에서 숫자야구게임을 직접 구현하였습니다.<br><br>
## 메인 시나리오
1. 컴퓨터의 숫자를 정하는 클래스 생성한다.<br><br>
2. 컴퓨터의 숫자를 정하는 클래스에서 1-9까지 서로 다른 3자리수를 생성하고<br>
   이 값을 반환하는 메소드 호출한다.<br><br>
3. InputView 클래스 생성한다.<br><br>
4. InputView에서 입력 안내 메세지를 출력하는 메소드를 호출한다.<br><br>
5. InputView에서 사용자로부터 3자리 숫자를 입력 받아<br>
   그 값을 반환하는 메소드를 호출한다.<br><br>
6. ResultView 클래스를 생성한다.<br><br>
7. ResultView에서 컴퓨터의 3자리 숫자와 사용자의 3자리 숫자를 매개변수로 받아서<br>
   서로 일치여부를 확인하는 메소드를 호출하고, 그 값을 반환한다.<br><br>
8. ResultView에서 일치여부(결과)를 콘솔창에 출력하는 메소드를 호출한다.

## 구현한 기능 목록
1. 1-9까지 서로 다른 수로 이루어진 난수를 만들어 반환하는 메소드<br><br>
2. 컴퓨터의 수와 사용자의 수 일치 여부를 확인하는 메소드

# 숫자야구게임 구현
위의 메인시나리오와 구현한 기능 목록을 토대로 숫자야구게임을 일단 구현하였습니다.<br><br>

## Main 클래스
```java
package NumberBaseballGame;

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        int index = 1;
        while(index != 2)
        {
            ComputerNumber computerNumber = new ComputerNumber();
            computerNumber.makeNumbers();
            InputView inputView = new InputView();
            NumberChecker numberChecker = new NumberChecker();
            ResultView resultView = new ResultView();
            while(numberChecker.getStrikeCount() < 3)
            {
                inputView.printMessage();
                UserNumber userNumber = new UserNumber(inputView.inputNumbers());
                numberChecker = new NumberChecker();
                numberChecker.checkNumber(computerNumber.getComputerNumbers(), userNumber.getUserNumbers());
                resultView.printMessage(numberChecker);
            }
            System.out.println("3개의 숫자를 모두 맞히셨습니다! 게임 종료\n" +
                    "게임을 새로 시작하려면 1, 종료하려면 2를 입력하세요.");
            Scanner scanner = new Scanner(System.in);
            index = scanner.nextInt();
        }
    }

}
```
index값을 1로 초기화하고 index가 2가 아닌동안 반복을 합니다.<br><br>
이는 나중에 숫자를 다 맞추고, 다시 시작할지 말지를 정할 때<br><br>
사용자가 2번을 입력하면 프로그램을 종료시키기 위한 코드입니다.<br><br>
반복문 내부에서 Computer클래스 객체를 생성하고,<br><br>
makeNumbers메소드를 호출하여 1~9까지 중복되지 않는 3가지 수를 생성합니다.<br><br>
사용자로부터 입력 안내메세지 출력과 숫자를 입력 받는 역할을 하는 InputView클래스의 객체를 생성합니다.<br><br>
컴퓨터의 숫자와 사용자의 숫자가 서로 일치하는지 어느 정도 일치하는지를 확인할 클래스인 NumberChecker클래스를 생성합니다.<br><br>
사용자가 입력한 숫자와 컴퓨터의 숫자가 얼마나 일치하는지 결과를 출력해주는 역할을 하는 resultView클래스를 생성합니다.<br><br>
스트라이크가 3개인 동안, 즉, 사용자가 입력한 숫자가 컴퓨터의 숫자와 값과 자리가 정확하게 일치하는 동안 반복합니다.<br><br>
반복문 내부에서 inputView객체의 printMessage메소드를 호출하여 숫자를 입력하라는 안내 메세지를 호출합니다.<br><br>
inputView객체의 inputNumbers메소드를 호출하여 사용자로부터 번호를 입력받고 이 번호를 int형으로 반환하는데<br><br>
이 값을 바탕으로 UserNumber클래스의 객체를 생성해줍니다.<br><br>
numberChecker를 새로 할당하여 strikeCount개수와 ballCount개수를 0으로 초기화해줍니다.<br><br>
그리고 numberChecker의 checkNumber메소드를 호출하여 매개변수로 computerNumber의 숫자와 useNumber의 숫자를 입력한 다음<br><br>
Strike와 ball의 개수를 세줍니다.<br><br>
resultView에서는 numberChecker를 매개변수로 입력 받아 printMessage를 호출하고,<br><br>
여기서 ball과 strike의 개수에 따라 알맞은 결과값을 콘솔창에 출력해줍니다.<br><br>
여기서 strike의 개수가 3개미만이면 다시 똑같은 과정을 반복하고,<br><br>
strike의 개수가 3개이면, 즉, 사용자의 입력 숫자가 컴퓨터의 모든 숫자와 값과 자리가 일치하면<br><br>
반복문을 종료하고 안내메세지를 출력하고,<br><br>
사용자가 1번을 입력하면 다시 반복을 하여 똑같은 과정을 반복하고<br><br>
2번을 누르면 프로그램을 종료시킵니다.<br><br>

## ComputerNumber 클래스
```java
package NumberBaseballGame;
import java.util.Set;
import java.util.LinkedHashSet;
import java.util.Random;
import java.util.List;
import java.util.ArrayList;

public class ComputerNumber {
    private List<Integer> computerNumbers;

    public void makeNumbers() {
        Random random = new Random();
        Set<Integer> numberSet = new LinkedHashSet<>();
        while(numberSet.size() < 3) {
            numberSet.add(random.nextInt(8) + 1);
        }
        computerNumbers = new ArrayList<>(numberSet);
    }

    public List<Integer> getComputerNumbers() { return computerNumbers; }
}
```
멤버로 List\<\Integer>의 객체인 computerNumbers를 가집니다.<br><br>
makeNumber메소드에서 Random클래스를 생성하고,<br><br>
Set의 객체인 numberSet을 LinkedList의 생성자로 생성합니다.<br><br>
numberSet의 size가 3보다 작은동안 반복합니다.<br><br>
반복문 내부에서 random객체의 nextInt(8)을 호출하여 0부터 8까지 무작위로 숫자를 받고,<br><br>
그 값에 1을 더해준 값을 numberSet에 add해줍니다.<br><br>
numberSet은 Set의 객체이기 때문에 Set의 특성상 중복되는 숫자는 저장할 수 없습니다.<br><br>
따라서 random으로부터 중복되지 않는 숫자 3개를 저장할 때까지 반복하다가<br><br>
중복되지 않는 숫자가 3개 저장되면 반복문이 끝나게 됩니다.<br><br>
반복문이 끝나면 이 numberSet의 객체와 ArrayList의 생성자로 멤버인 computerNumber를 생성합니다.<br><br>

## InputView 클래스
```java
package NumberBaseballGame;

import java.util.Scanner;

public class InputView {
    public void printMessage() {
        System.out.print("숫자를 입력해 주세요 : ");
    }
    public int inputNumbers() {
        return new Scanner(System.in).nextInt();
    }
}
```
사용자로부터 숫자를 입력하라고 안내메세지를 콘솔창에 출력하는 printMessage메소드와<br><br>
콘솔창에 사용자로부터 입력 받은 숫자를 int형으로 반환하는 inputNumbers메소드를 정의합니다.

## UserNumber 클래스
```java
package NumberBaseballGame;

import java.util.ArrayList;
import java.util.List;

public class UserNumber {
    private List<Integer> userNumbers;

    public UserNumber(int userNumbers) {
        this.userNumbers = new ArrayList<>();
        String strNumbers = String.valueOf(userNumbers);
        for(int i = 0; i < strNumbers.length(); i++) {
            this.userNumbers.add(Integer.parseInt(String.valueOf(strNumbers.charAt(i))));
        }
    }

    public List<Integer> getUserNumbers() { return userNumbers; }
}
```
멤버로는 List\<Integer\>의 객체인 userNumbers를 가집니다.<br><br>
생성자에서 사용자로부터 입력 받은 숫자인 int형 매개변수를 먼저 문자열로 바꿔줍니다.<br><br>
문자열의 길이동안 반복을 하면서 문자를 추출하고 그 문자를 문자열로 바꾼다음 다시 Integer로 바꿔서 userNumbers에 add해줍니다.<br><br>

## NumberChecker 클래스
```java
package NumberBaseballGame;

import java.util.List;

public class NumberChecker {
    private int strikeCount;
    private int ballCount;

    public NumberChecker() {
        strikeCount = 0;
        ballCount = 0;
    }

    public void checkNumber(List<Integer> computerNumbers, List<Integer> userNumbers) {
        for(int i = 0; i < userNumbers.size(); i++) {
            for(int j = 0; j < computerNumbers.size(); j++) {
                if(userNumbers.get(i) == computerNumbers.get(j)) {
                    if(i == j) {
                        strikeCount++;
                        break;
                    }
                    ballCount++;
                    break;
                }
            }
        }
    }

    public int getStrikeCount() { return strikeCount; }

    public int getBallCount() { return ballCount; }
}
```
멤버로 int형인 strikeCount와 ballCount를 가집니다.<br><br>
생성자에서 이 두 멤버의 값을 0으로 초기화해줍니다.<br><br>
checkNumber메소드에서 컴퓨터의 숫자리스트와 사용자의 숫자리스트를 매개변수로 입력 받아<br><br>
이중반복을 돌리면서 컴퓨터의 숫자리스트의 원소값과 사용자의 숫자리스트 원소값이 같은지 확인합니다.<br><br>
원소값이 같으면 원소의 위치가 같은지도 확인합니다.<br><br>
원소갑과 위치 둘 다 같으면 strikeCount를 증가시켜주고 원소값만 같으면 ballCount를 증가시켜 줍니다.<br><br>

## ResultView 클래스
```java
package NumberBaseballGame;

public class ResultView {
    public void printMessage(NumberChecker numberChecker) {
        if(numberChecker.getBallCount() == 0 &&
                numberChecker.getStrikeCount() == 0) {
            System.out.println("낫싱");
            return;
        }
        if(numberChecker.getBallCount() == 0 &&
                numberChecker.getStrikeCount() > 0) {
            System.out.printf("%d스트라이크\n", numberChecker.getStrikeCount());
            return;
        }
        if(numberChecker.getBallCount() > 0 &&
                numberChecker.getStrikeCount() == 0) {
            System.out.printf("%d볼\n", numberChecker.getBallCount());
            return;
        }
        if(numberChecker.getBallCount() > 0 &&
                numberChecker.getStrikeCount() > 0) {
            System.out.printf("%d볼 %d스트라이크\n",
                    numberChecker.getBallCount(), numberChecker.getStrikeCount());
        }
    }
}
```
printMessage에서 결과값을 출력해줘야 하는데 이때 매개변수로 입력받은 numberChecker의<br><br>
getStrikeCount와 getBallCount메소드를 통해 strikeCount와 ballCount를 구합니다.<br><br>
if문을 통해 strikeCount와 ballCount에 따라 다른 메세지를 콘솔창에 출력합니다.

## 아쉬운 점
어거지로 돌아가는 프로그램을 구현하였으나 테스트는 아무것도 하지 못하였고<br><br>
else는 사용하지 않았으나 1단계 들여쓰기 규칙을 어겼습니다.<br><br>
이는 아무래도 적절한 클래스 생성과 메소드 분리에 실패했기 때문입니다.<br><br>
또한 사용자가 잘못된 숫자를 입력할 것이라는 가정을 아예 하지 않아서 이에 대한 예외처리를 하지 못했습니다.
