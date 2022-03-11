# 4일에 공부한 내용를 적었습니다.
<a href="https://homoefficio.github.io/2019/10/03/Java-Optional-%EB%B0%94%EB%A5%B4%EA%B2%8C-%EC%93%B0%EA%B8%B0/" target="_blank">Java Optional 바르게 쓰기</a>를 참고하였습니다.<br><br>
Optional클래스는 메서드가 반환할 결과값이 '없음'을 명백하게 표현할 필요가 있고,<br><br>
null을 반환하면 에러가 발생할 확률이 높은 상황에서 메서드의 반환 타입으로 Optional을 사용하는 것이<br><br>
Optional 클래스를 만든 주 목적입니다.<br><br>
Optional타입의 변수 값은 절대 null이면 안되고,<br><br>
항상 Optional인스턴스를 가리켜야 합니다.<br><br>
## 1. isPresent() + get() 대신에 orElse() / orElseGet() / orElseThrow()를 사용하자.
Optional클래스를 사용하는 것은 비쌉니다.<br><br>
비싸다는 말은 일단 Optional클래스를 사용하면 메모리를 많이 차지합니다.<br><br>
속도도 아무래도 늦어질 수 있습니다.<br><br>
그래서 굳이 Optional클래스를 쓸거면 코드양이라도 줄일 수 있어야 합니다.<br><br>
```java
//안좋은 예시
Optional<Member> member = ...;
if(member.isPresent()) {
    return member.get();
} else {
    return null;
}

//좋은 예시
Optional<Member> member = ...;
return member.orElse(null);

//안좋은 예시
Optional<Member> member = ...;
if(member.isPresent()) {
    return member.get();
} else {
    throw new NoSuchElementException();
}

//좋은 예시
Optional<Member> member = ...;
return member.orElseThrow(() -> new NoSuchElementException());
```

## 2. orElse(new ...) 대신에 orElseGet(() -> new ...)를 사용하자.
orElse(...)에서 ...은 Optional에 값이 있든 없든(null이든 아니든) 무조건 실행됩니다.<br><br>
따라서 만약에 ...이 새로운 객체를 생성하거나 새로운 연산을 수행하는 경우에는<br><br>
orElse(...)대신에 orElseGet(...)을 사용해야 합니다.<br><br>
orElse(...)는 아까 말했듯이 무조건 ...이 실행되고<br><br>
orElseGet(...)은 Optional에 값이 있으면 실행이 안되고 값이 없는 경우에만 실행됩니다.<br><br>
이는 기본적인 내용인데 일단 orElse(...)가 값이 있든 없든 무조건 실행이 되기 때문에<br><br>
예를 들어 method1(method2())이면 method2는 항상 method1보다 먼저 그리고 언제나 실행이 됩니다.<br><br>
따라서 orElse(new ...)에서도 new ...이 항상 실행이 됩니다.<br><br>
Optional에 값이 없는 경우에는 orElse()에서 실행한 값이 반환되기 때문에 실행한 의미가 있지만<br><br>
Optional에 값이 있는 경우에는 orElse에서 실행한 값은 무시되고 버려집니다.<br><br>
따라서 orElse(...)에서 ...은 새 객체 생성이나 새로운 연산을 유발하지 않고,<br><br>
이미 생성되었거나 계산되어 있는 값일 때만 사용해야 합니다.<br><br>
이와 반대로 orElseGet(Supplier)에서 Supplier는 Optional에 값이 없을 때만 실행됩니다.<br><br>
따라서 Optional에 값이 없을 때만 새로운 객체를 생성하거나 새로운 연산을 실행하기 때문에 불필요하게 버려지는 것이 없습니다.<br><br>
```java
//안좋은 예시
Optional<Member> member = ...;
return member.orElse(new Member()); //member에 값이 있든 없든 new Member()는 무조건 실행되기 때문에 불필요한 낭비 발생 

//좋은 예시
Optional<Member> member = ...;
return member.orElseGet(Member::new);//member에 값이 없을 때만 new Member()가 실행되기 때문에 불필요한 낭비가 없음

//좋은 예시
Member EMPTY_MEMEBER = new Member();
...
Optional<Member> member = ...;
return member.orElse(EMPTY_MEMEBER);//이미 생성되었거나 계산된 값은 orElse를 이용해도 불필요한 낭비가 없어서 괜찮음
```
참고로 Collections.emptyList()는 호출할 때마다 비어있는 리스트를 새로 생성하여 반환하는 것이 아니라<br><br>
이미 생성된 static 변수인 EMPTY_LIST를 반환하기 때문에 orElse(Collectoins.emptyList())를 써도 괜찮습니다.<br><br>
하지만 이런 용법을 사용하다보면 orElse(new ...)나 orElse(새로은 연산을 유발하는 메소드)들도 정상적인 사용법으로 인식되게 하기 때문에<br><br>
orElseGet(Collections.emptyList())를 사용하는 것이 더 좋습니다.<br><br>

## 3. 단지 값을 얻을 목적이라면 Optional 대신에 null 비교를 사용하자.
아까도 말했듯이 Optional은 비쌉니다.<br><br>
그래서 단순히 값 또는 null을 얻을 목적이라면 Optional대신에 null비교를 사용하는 것이 좋습니다.<br><br>
```java
//안좋은 예시
return Optional.ofNullable(status).orElse(READY);

//좋은 예시
return status != null ? status : READY;
```
ofNullble인 경우 status가 null인지 값이 있는지 알 수 없는 경우 사용하는데 값이 있으면 status가 반환이 될 것이고,<br><br>
값이 없다면, 즉, status가 null이면 orElse의 반환값인 READY가 반환됩니다.<br><br>
이 경우 Optional을 사용하지 말고 삼항연산자를 이용하여<br><br>
status가 null이 아니면 status값을 그대로 반환하고,<br><br>
status가 null이면 READY를 반환하도록 합니다.<br><br>

## 4. Optional 대신에 비어있는 컬렉션을 반환하자.
Optional은 비쌉니다.<br><br>
그리고 컬렉션은 null이 아니라 비어 있는 컬렉션을 반환하는 것이 좋을 때가 많습니다.<br><br>
따라서 컬렉션을 Optional로 감싸서 반환하지 말고, 비어있는 컬렉션을 반환하는 것이 좋습니다.<br><br>
```java
//안좋은 예시
List<Member> members = team.getMembers();
return Optional.ofNullable(members);

//좋은 예시
List<Member> members = team.getMembers();
return members != null ? members : Collections.emptyList();
```

## 5. Optional을 필드로 사용하지 말자.
Optional은 필드로 사용할 목적으로 만들어지지 않았으며, Serializable을 구현하지 않았습니다.<br><br>
따라서 Optional을 필드로 사용하면 안됩니다.<br><br>
```java
//안좋은 예시
public class Member{
    private Long id;
    private String name;
    private Optional<String> email = Optional.empty();
}

//좋은 예시
public class Member{
    private Long id;
    private String name;
    private String email;
}
```
## 6. Optional을 생성자나 메소드 인자로 사용하지 말자.

Optional을 생성자나 메소드 인자로 사용하면 생성자나 메소드를 호출할 때마다 Optional을 생성해서 인자로 전달해야 합니다.<br><br>
하지만 생성자나 메소드 내에서는 인자가 null인지 아닌지 항상 체크하는 것이 안전합니다.<br><br>
그렇기 때문에 굳이 비싼 Optional을 사용하지 말고, 메소드 내부에서 null체크를 하는 것이 좋습니다.<br><br>
```java
//안좋은 예시
public class HRManager {
    public void increaseSalary(Optional<Memeber> member){
        member.ifPresent(member -> member.increaseSalary(10));
    }
}
hrManager.increaseSalary(Optional.ofNullable(member));

//좋은 예시
public class HRManager {
    public void increaseSalary(Memeber member) {
        if(member != null) {
            member.increaseSalary(10);
        }
    }
}
hrManager.increaseSalary(member);
```
인자로 넣을 때 ofNullable에 넣어서 넘기지 말고 애초에 member 그대로 넘기고 내부에서 null인지 체크를 하는 것이 더 좋습니다.<br><br>

## 7. Optional을 컬렉션의 원소로 사용하지 말자.
컬렉션에는 많은 원소가 들어갈 수 있기 때문에 비싼 Optional을 원소로 사용할 필요가 없습니다.<br><br>
원소를 꺼낼 때나 사용할 때 null을 체크하는 방법이 더 좋고,<br><br>
특히 Map의 경우 getOrDefault(), putIfAbsent(), computeIfAbsent(), computeIfPresent()처럼 null체크가 포함된 메서드를 제공하기 때문에<br><br>
Map의 원소로 Optional을 사용하는 것보다 Map이 제공하는 null체크 메소드를 사용하는 편이 더 좋습니다.<br><br>
```java
//안좋은 예시
Map<String, Optional<String>> sports = new HashMap<>();
sports.put("100", Optional.of("BasketBall"));
sports.put("101", Optional.ofNullable(someOtherSports));
String basketBall = sports.get("100").orElse("BasketBall");
string unknown = sports.get("101").orElse("");

//좋은 예시
Map<String, String> sports = new HashMap<>();
sports.put("100", "BasketBall");
sports.put("101", null);
String basketBall = sports.getOrDefault("100", "BasketBall");
Stirng unknow = sports.computeIfAbsent("101", k -> "");
```

## 8. of()와 ofNullable()을 사용하는 경우를 혼동하지 말자.
of(X)는 X가 null이 아님이 확실할 때만 사용하며, 만약에 X가 null이면 NullPointerExceptoin이 발생합니다.<br><br>
ofNullable(X)는 X가 null인지 아닌지 불확실할 때만 사용하며, X가 확실히 null이 아니라면 of(X)를 사용해야 합니다.<br><br>
```java
//안좋은 예시
return Optional.of(member.getEmail());//member의 email이 null이면 NullPointerExceptoin 발생

//좋은 예시
return Optional.ofNullable(member.getEmail());

//안좋은 예시
return Optional.ofNullable("READY");//"READY"는 확실히 null이 아니기 때문에 of을 사용해야함.

//좋은 예시
return Optional.of("READY");
```

## 9. Optional\<T\> 대신에 OptionalInt, OptionalLong, OptionalDouble을 사용하자.
Optional에 담길 값이 int, long, double이라면 boxing/unboxing이 발생하는<br><br>
Optional\<Integer\>, Optional\<Long\>, Optional\<Double\>을 사용하지 말고,<br><br>
OptionalInt, OptionalLong, OptionalDouble을 사용하는 것이 좋습니다.<br><br>
```java
//안좋은 예시
Optional<Integer> count = Optinal.of(38);//boxing발생
for(int i = 0; i < count.get(); i++) { //unboxing발생
...
}

//좋은 예시
OptionalInt count = OptionalInt.of(38);//boxing발생 안함
for(int i = 0; i < count.getAsInt(); i++) { //unboxing발생 안함
...
}
```
