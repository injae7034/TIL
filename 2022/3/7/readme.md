# 7일에 공부한 내용을 적었습니다.

## Maven, Gradle
Maven이나 Gradle은 빌드도구입니다.<br><br>
Maven이나 Gradle같은 빌드도구가 없을 때는 사용자가 직접 External Libraries에 직접 자료파일(압축zip파일)을 추가해줘야 했습니다.<br><br>
또한 버전이 업데이트 되면 기존 버전을 지우고 새로 업데이트된 버전을 External Libraries에 추가하는 이 모든 과정을 사용자가 직접해야 했습니다.<br><br>
그래서 이 귀찮은 과정을 대신 처리해주는 Maven이나 Gadle과 같은 빌드도구들이 등장하게 되었습니다.<br><br>
```java
repositories {
    mavenCentral()
}

dependencies {
    testCompile('org.junint.jupiter:junit-jupiter:5.7.1')
    testCompile('org.assertj:assertj-core:3.19.0')
}
```
Gradle에 dependencies의 testCompile에 필요한 것들을 명시해주면 Gradle이라는 도구가 그것들을 알아서 External Libraries에 추가해줍니다.<br><br>
버전 코드를 수정하면 수정된 버전을 External Libraries에 추가해줍니다.<br><br>
이러한 것들을 repositories(저장소)의 mavenCentral에서 testCompile에 명시된 것들을 다운로드 받아 옵니다.<br><br>
그래서 Maven이나 Gradle같은 도구를 사용하면 이러한 빌드도구들이 프로젝트 구조, 뼈대를 잡아 줍니다.<br><br>
Maven이나 Gradle로 부터 main과 test라는 2개의 Directory가 자동으로 생성됩니다.<br><br>

## main, test
main에는 실제 프로그래머가 배포할 코드, 즉, 서비스하고자 하는 코드가 들어가고,<br><br>
test에는 실제 서비스에 영향을 주지 않는 코드, 즉, 주로 테스트하는 코드들이 들어갑니다.<br><br>
이 때 테스트에는 A-Z까지 다 만든 다음에 테스트하는 코드가 들어가는 것이 아니라<br><br>
사실은 Production, 즉, 실제 프로그래머가 배포할 코드들을 테스트하기 위한 코드들이 test에 들어갑니다.<br><br>

## main 메소드
자바는 기본적으로 무조건 클래스가 필요합니다.<br><br>
자바는 함수가 단독으로 실행이 될 수 없고, 무조건 클래스에 있는 메소드를 실행해야 합니다.<br><br>
그래서 main메소드를 담고 있는 클래스를 생성해야하는데 이는 본인이 원하는 클래스명을 지어서 생성하면 됩니다.<br><br>
클래스를 생성한 다음 프로그램의 시작점으로 main메소드를 생성해줘야 합니다.<br><br>
IntellJ를 사용할 경우 psvm 또는 main을 치면 자동으로 main 메소드가 완성이 됩니다.<br><br>
이 main메소드가 프로그램의 시작점이 됩니다.<br><br>

## package
관련있는 클래스파일들을 한 곳에 모으는 기능을 합니다.<br><br>
관련있는 클래스파일들을 한 곳에 모아두는 패키지를 활용하면 클래스명들을 다보지 않고 패키지명만 보더라도 이게 무슨 프로그램인지 알 수 있습니다.<br><br>
자바는 Package는 Directory와 같은 의미라고 생각하면 됩니다.<br><br>
그래서 main디렉토리의 java디렉토리에 package baseball을 추가하명 java디렉토리 아래에 다시 baseball디렉토리가 생성되고,<br><br>
main메소드는 이 baseball디렉토리에 속하게 됩니다.<br><br>
그래서 항상 프로그램을 구현할 때 항상 package에 어떤 프로그램을 만들지에 대한 명칭을 적어주는 것이 좋습니다.<br><br>
예를 들어, 숫자야구게임 프로그램을 구현할 때는 package에 baseball이라고 적어주면 됩니다.<br><br>
즉, package의 명칭을 내가 만드려는 프로그램과 관련이 있는 명칭으로 지어주는 것이 좋습니다.<br><br>
그래야 package명칭만 보더라도 이 프로그램이 무엇과 관련이 있는지 바로 알 수 있습니다.<br><br>

## domain
내가 실제 다루고자 하는 서비스의 비지니스 로직이 들어 있는 영역입니다.<br><br>
baseball 디렉토리 아래에 domain디렉토리를 생성합니다.<br><br>
즉, 이 domain 디렉토리는 숫자야구와 관련이 있다는 것을 알 수 있습니다.<br><br>
여기에 숫자야구게임의 비지니스로직과 관련된 클래스를 생성해서 한 곳에 관리합니다.
Ball이라는 클래스를 생성하면 아래와 같은 package를 가집니다.<br><br>
```java
package baseball.domain

public class Ball{

}
```

## final
자바에서 상수는 모든 문자가 대문자로 사용하기 때문에 단어가 결합된 상태에서는 \_로 구분해줍니다.

## 객체 지향 프로그래밍
1. 기능을 가지고 있는 클래스를 인스턴스화(=객체)한다.<br><br>
2. 필요한 기능을 (역할에 맞는) 인스턴스가 수행하게 한다.(=의인화)<br><br>
3. 각 결과를 종합한다.<br><br>

##  Ctrl + Alt + l
IntelliJ에서 Ctrl + Alt + l을 누르면 자동으로 줄 간격 조절을 해줍니다.<br><br>
클래스 내부에 마우스 커서가 있는 상태에서는 그 클래스만 줄 간격이 조절되고<br><br>
패키지를 마우스로 클릭한 다음에 Ctrl + Alt + l을 눌러주면 패키지 내의 모든 클래스의 줄 간격이 자동으로 조절됩니다.<br><br>
