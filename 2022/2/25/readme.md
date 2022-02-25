# 25일에 공부한 내용을 적었습니다.

# 객체지향 생활 체조 원칙 9가지
<ol>
  <li>한 메서드에 오직 한 단계의 들여쓰기만 한다.</li>
  <li>else 예약어는 쓰지 않는다.</li>
  <li>모든 원시 값과 문자열을 포장한다.</li>
  <li>한 줄에 점을 하나만 찍는다.</li>
  <li>줄여 쓰지 않는다.(축약금지)</li>
  <li>모든 엔티티를 작게 유지한다.</li>
  <li>3개 이상의 인스턴스 변수를 가진 클래스를 쓰지 않는다.</li>
  <li>일급 컬렉션을 쓴다.</li>
  <li>getter/setter/프로퍼티를 쓰지 않는다.</li>
</ol>
여기서 지금 이해가 가는 건 한 메서드에 오직 한 단계의 들여쓰기만 하는 것과<br><br>
else예약어를 사용하지 않는다는 것입니다.<br><br>
차차 다음 미션을 하다보면 나머지도 이해가 가리라 생각하고,<br><br>
일단은 한 단계의 들여쓰기와 else 예약어를 사용하지 않도록 노력해야겠습니다.<br><br>

# Clean Code 가이드
## 함수(메소드)
<ul>
  <li>작게 만들어라.</li>
  <li>한 가지만 해라.</li>
  <li>함수 당 추상화 수준은 하나로</li>
  <li>서술적인 이름을 사용하라.</li>
  <li>함수인수는 무항, 1개, 2개 추천 3개는 가급적 피하고, 4개 이상은 무조건 피해라.</li>
  <li>side effect를 만들지 마라.</li>
  <li>명령과 조회를 분리하라.</li>
  <li>오류 코드보다 예외를 사용하라.</li>
  <li>반복하지 마라.</li>
</ul>

메소드를 만들 때 가능한 작게 그러려면 당연히 한 가지만 해야합니다.<br><br>
서술적인 이름을 사용하여 다른 사람도 이해하기 쉽게 명칭을 지어야 하고,<br><br>
함수의 인수는 작을수록 좋으니까 최대한 줄이도록 노력하고,<br><br>
명령과 조회를 한 메소드에서 하지 말고 분리시켜야 하며<br><br>
반복되는 것을 최대한 줄이기 위해 노력해야 합니다.<br><br>
나머지 사항들은 현재 확 와닿지는 않는데 미션을 진행하다 보면 이해할 수 있을것이라 생각합니다.<br><br>

# 단위테스트
## main method
지금까지 C와 C++로 데스크톱 애플리케이션을 만들 때, java로 데스크톱 프로젝트를 옮길 때에<br><br>
모두 main메소드에서 콘솔창에 직접 값을 출력해보면서 내가 구현한 메소드들이 제대로 돌아가고 있는지 확인하였는데<br><br>
자바에는 JUnit이라는 도구가 있어 이를 대체할 수 있습니다.<br><br>
이를 활용하면 main메소드를 이용하여 단위테스트를 할 때 보다 훨씬 편하기 테스트를 할 수 있습니다.

# 학습테스트 실습
## String 클래스에 대한 학습테스트
JUnit에서 assertj를 활용하면 좀 더 직관적으로 테스트를 실행할 수 있습니다.<br><br>
기존에 JUnit에서는 인수를 2개 입력 받아서 예를 들어 A와 B를 입력 받아 2개를 비교하는데<br><br>
그 순서를 헷갈려서 반대로 넣는 경우가 발생할 수 있는데<br><br>
assertj는 메서드 체이닝을 지원해, 더 직관적이고 읽기 쉬운 테스트코드 작성 가능합니다.<br><br>
```java
package study;

import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;

public class StringTest {
    @Test
    void replace() {
        String actual = "abc".replace("b", "d");
        assertThat(actual).isEqualTo("adc");
    }
}
```
위의 코드는 replace메소드가 제대로 돌아가고 있는지 확인하는 테스트코드인데<br><br>
replace메소드에 위에 test annotation을 통해 테스트할 단위를 지정합니다.<br><br>
"abc"문자열에서 "b"를 "d"로 replace한 다음에 actual에 저장합니다.<br><br>
이 후 assertj의 assertThat을 이용해 actual이 "adc"와 같은지(isEqualTo) 확인하고<br><br>
예상하는 것과 실제가 일치하기 때문에 테스트를 실행하면 아무 문제없이 돌아갑니다.<br><br>

<ul>요구사항 1
  <li>"1,2"을 ,로 split 했을 때 1과 2로 잘 분리되는지 확인하는 학습 테스트를 구현한다.</li>
  <li>"1"을 ,로 split 했을 때 1만을 포함하는 배열이 반환되는지에 대한 학습 테스트를 구현한다.</li>
</ul>
<br><br>
요구사항에 맞게 코드를 짜면

```java
 @Test
    public void split() {
        String[] values = "1,2".split(",");
        assertThat(values).contains("1");
        assertThat(values).contains("2");
        assertThat(values).contains("1", "2");
        assertThat(values).contains("2", "1");
        //assertThat(values).containsExactly("1");//false
        //assertThat(values).containsExactly("2");//false
        //assertThat(values).containsExactly("2", "1");//false
        assertThat(values).containsExactly("1", "2");
        values = "1".split(",");
        assertThat(values).contains("1");
        assertThat(values).containsExactly("1");
    }
```

"1,2"를 split(",")을 하면 String배열에는 "1"과 "2"가 원소로 저장됩니다.<br><br>
여기서 contains는 문자열 배열에 "1"이나 "2" 둘 중에 하나라도 있는지 확인이 가능합니다.<br><br>
즉, contains는 문자열 배열에 개별 원소가 있는지 확인하는 것이라 매개변수로 "1" 혼자 들어가도 되고<br><br>
"2"혼자 들어가도 되고, "1", "2" 둘다 들어가도 되고, 순서를 바꿔 "2", "1"이 들어가도 모두 true입니다.<br><br>
하지만 containsExactly는 정확하게 일치하는 경우에만 true이기 때문에 "1"이나 "2"가 하나만 매개변수로 들어가면 false이고<br><br>
"2", "1" 이렇게 순서가 바뀌어도 false입니다.<br><br>
"1", "2" 이렇게 순서까지 정확하게 일치해야만 true입니다.<br><br>

<ul>요구사항 2
  <li>"(1,2)" 값이 주어졌을 때 String의 substring() 메소드를 활용해 ()을 제거하고 "1,2"를 반환하도록 구현한다.</li>
</ul>

```java
@Test
    public void substring() {
        String value = "(1,2)".substring(1, 4);
        assertThat(value).contains("1");
        assertThat(value).contains("1", ",");
        assertThat(value).contains("1", ",", "2");
        assertThat(value).contains(",", "2");
        assertThat(value).contains("2");
        assertThat(value).contains("1", "2");
        assertThat(value).contains(",");
    }
```
다음은 substring을 테스트하는 코드인데 "(1,2)"에서 substring(1,4)를 하면<br><br>
value에는 "1,2"가 저장됩니다.<br><br>
substring의 반환값인 value는 String이기 때문에 contains만 사용이 가능하고,<br><br>
containsExactly는 String배열에만 사용할 수 있기 때문에 사용할 수 없습니다.<br><br>
contains로 순서와 개수와 상관없이 "1", ",", "2"가 contains되있는지 확인하면 모두 true입니다.<br><br>

<ul>요구사항 3
  <li>"abc" 값이 주어졌을 때 String의 charAt() 메소드를 활용해 특정 위치의 문자를 가져오는 학습 테스트를 구현한다.</li>
  <li>String의 charAt() 메소드를 활용해 특정 위치의 문자를 가져올 때 위치 값을 벗어나면 StringIndexOutOfBoundsException이 발생하는 부분에 대한 학습 테스트를 구현한다.</li>
  <li>JUnit의 @DisplayName을 활용해 테스트 메소드의 의도를 드러낸다.</li>
</ul>

```java
    @Test
    @DisplayName("StringIndexOutOfBoundsException 테스트")
    public void charAt() {
        String value = "abc";

        assertThatThrownBy(() -> {
            value.charAt(3);
        }).isInstanceOf(IndexOutOfBoundsException.class)
                        .hasMessageContaining("String index out of range: 3");


        assertThatExceptionOfType(IndexOutOfBoundsException.class)
                .isThrownBy(() -> {
                    value.charAt(3);
                }).withMessageMatching("String index out of range: 3");
    }
```
DisplayName이라는 annotation을 활용해 무엇에 대한 테스트인지 명시할 수 있습니다.<br><br>
여기서는 "StringIndexOutOfBoundsException 테스트"라고 명시합니다.<br><br>
value에 "abc"가 저장되어 있을 때, charAt(3)를 호출하면 StringIndexOutOfBoundsException가 발생합니다.<br><br>

