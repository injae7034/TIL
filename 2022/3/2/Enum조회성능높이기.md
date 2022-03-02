# Enum 조회시 성능 높이기

<a href="https://pjh3749.tistory.com/279" target="_blank">Enum 조회 성능 높여보기</a>를 참고하였습니다.<br><br>

자바에서 enum을 구현한 후에 보통 find메소드를 통해 알맞는 상수를 찾는 로직을 많이 구현합니다.<br><br>

```java
import java.util.stream.Stream;

import lombok.Getter;

@Getter
public enum AccountStatus {
    INUSE("사용중"),
    UNUSED("미사용"),
    DELETED("삭제"),
    UNKNOWN("알수없음");
    
    private final String description;
    
    AccountStatus(String description) {
        this.description = description;
    }
    
    public static AccountStatus find(String description) {
        return Arrays.stream(values())
            .filter(accountStatus -> accountStatus.descripiton.equals(description))
            .findAny()
            .orElse(UNKNOWN);
    }
}
```
이 find 메소드는 enum의 상수값을 스트림화한다음에 필터에<br><br>
매개변수로 입력 받은 문자열인 description과 일치하는지 predicate식을 넣어 주고,<br><br>
찾은게 있으면 찾은 원소를 반환하는데 원소의 자료형이 AccountStatus, 즉, enum의 객체를 반환하고,<br><br>
찾은게 없으면 UNKNOWN을 반환하는 구조입니다.<br><br>

다른 방법으로는 Streams의 of을 이용하는 방법이 있습니다.<br><br>
```java
public static AccountStatus find(Stirng description) {
    return Stream.of(values())
        .filter(accountStatus -> accountStatus.description.equals(description))
        .findAny()
        .orElse(UNKNOWN);
}
```
또 다른 방법으로는 HashMap을 이용하는 방법이 있습니다.<br><br>
```java
@Getter
public enum AccountStatus {
    INUSE("사용중"),
    UNUSED("미사용"),
    DELETED("삭제"),
    UNKNOWN("알수없음");
    
    private final String description;
    
    AccountStatus(String description) {
        this.description = description;
    }
    
    private static final Map<String, AccountStatus> descriptions = 
          Collections.unmodifiableMap(Stream.of(values())
            .collect(Collectors.toMap(AccountStatus::getDescription, Function.identity())));
    
    public static AccountStatus find(String description) {
        return Optional.ofNullable(descriptions.get(description)).orElse(UNKNOWN);
    }
}
```
수정 불가능한 Map을 선언하고 Collections의 unmodifiableMap을 통해 수정불가능한 Map의 객체를 반환받아 이를 저장합니디.<br><br>
unmodifiableMap의 매개변수로 열거형 상수들을 stream화한 다음에 collect메소드에 매개변수로<br><br>
Collectors.toMap을 넣습니다.<br><br>
key값으로는 열거형 상수의 멤버인 description문자열을 넣고, value값은 Function.identity가 들어가는데<br><br>
의미는 그냥 넣은 값이 그대로 출력된다는 말입니다.<br><br>
현재 final Map의 value값은 AccountStatus, 즉, enum클래스이기 때문에<br><br>
value값은 enum상수형 값이 그대로 들어간다고 보면 됩니다.<br><br>
결과적으로 find에서는 이렇게 수정불가능하게 만들어진 맵을 이용하여<br><br>
key값(description)으로 바로 value(Enum상수)를 얻을 수 있기 때문에 속도가 매우 빠릅니다.<br><br>
find에서 찾는 것이 없을 경우 null일 수 있기 때문에 Optional클래스를 이용하여 ofNullable메소드에 value값을 호출합니다.<br><br>
즉, 위의 Arrays.stream이나 Stream.of을 이용한 방법은 find를 호출할 때마다 매번 stream을 해야 하기 때문에 시간 낭비가 심합니다.<br><br>
하지만 맵을 이용하면 최초에 enum이 생성될 때 한번만 stream을 열어서 맵을 생성해주고<br><br>
find에서는 이미 생성된 map을 매번 활용하기 때문에 시간과 자원면에서 훨씬 더 효율적이라고 할 수 있습니다.<br><br>
