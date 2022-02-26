# 26일에 공부한 내용을 적었습니다.

# Set Collection에 대한 학습테스트

다음과 같이 코드가 주어졌을 때 요구사항을 만족해야 합니다.
```java
public class SetTest {
    private Set<Integer> numbers;

    @BeforeEach
    void setUp() {
        numbers = new HashSet<>();
        numbers.add(1);
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);
    }
    
    // Test Case 구현
}
```

BeforeEach annotation은 각 테스트 함수가 호출되기 전에<br><br>
매번 BeforeEach annotation이 있는 테스트 함수가 먼저 호출되도록 합니다.<br><br>
이와 달리 BeforeAll은 테스트 함수가 불리기 전에 딱 한 번 호출 됩니다.<br><br>
즉, BeforeEach를 통하여 어떠한 테스트 함수를 실행하기 전에<br><br>
SetTest의 numbers멤버를 HashSet생성자로 생성한 뒤에 1, 1, 2, 3을 add하는 초기화과정을 거칩니다.<br><br>

<ul>요구사항 1
  <li>Set의 size() 메소드를 활용해 Set의 크기를 확인하는 학습테스트를 구현한다.</li>
</ul>

```java
@Test
    public void size() {
        assertThat(numbers.size()).isEqualTo(3);
        //assertThat(numbers.size()).isEqualTo(4);
    }
```

assertThat에 numbers의 size메소드를 매개변수로 넣고, isEqualTo(3)을 호출하면 정상적으로 원하는 결과를 얻을 수 있습니다.<br><br>
그러나 4를 넣으면 test에서 기대값 4가 실제값 3과 다르다고 출력이 됩니다.<br><br>
이는 numbers의 클래스가 HashSet인데 HashSet은 중복을 허용하지 않기 때문에 중복으로 들어간 1을 하나만 저장하여 결과적으로 size는 3입니다.<br><br>

<ul>요구사항 2
  <li>Set의 contains() 메소드를 활용해 1, 2, 3의 값이 존재하는지를 확인하는 학습테스트를 구현하려한다.</li>
  <li>구현하고 보니 다음과 같이 중복 코드가 계속해서 발생한다.</li>
  
  ```java
    @Test
    void contains() {
        assertThat(numbers.contains(1)).isTrue();
        assertThat(numbers.contains(2)).isTrue();
        assertThat(numbers.contains(3)).isTrue();
    }
  ```
  
  <li>JUnit의 ParameterizedTest를 활용해 중복 코드를 제거해 본다.</li>
</ul>

```java
    @DisplayName("true만 리턴")
    @ParameterizedTest
    @ValueSource(ints = {1, 2, 3})
    public void setContainsTest_onlyTrue(int element) {
        assertThat(numbers.contains(element)).isTrue();
    }
```
ParameterizedTest와 ValueSource를 이용하면 위와 같이 중복없이 한 줄로 HashSet의 모든 원소가 있는지 확인할 수 있습니다.<br><br>
대신 ints에 원소를 넣어줘야 합니다.<br><br>

<ul>요구사항 3
  <li>요구사항 2는 contains 메소드 결과 값이 true인 경우만 테스트 가능하다. 입력 값에 따라 결과 값이 다른 경우에 대한 테스트도 가능하도록 구현한다.</li>
  <li>예를 들어 1, 2, 3 값은 contains 메소드 실행결과 true, 4, 5 값을 넣으면 false 가 반환되는 테스트를 하나의 Test Case로 구현한다.</li>
</ul>

```java
    @DisplayName("true, false 같이 리턴")
    @ParameterizedTest
    @CsvSource(value = {"1 : true", "2 : true", "3 : true", "4 : false", "5 : false"},
            delimiter = ':')
    public void setConstainsTest_trueOrFalse(int element, boolean expected) {
        assertThat(numbers.contains(element)).isEqualTo(expected);
    }
```
CsvSource를 이용하고 delimiter값으로 구분자를 설정하여 value에 1, 2, 3은 true값이 나오도록 저장하고,<br><br>
4, 5값은 false가 나오도록 저장하면 테스트에서 1, 2, 3은 true가 출력되고, 4, 5는 false가 출력됩니다.
