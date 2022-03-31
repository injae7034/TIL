# 29일에 공부한 내용을 적었습니다.
## Gradle이란 무엇인가?
Gradle은 빌드 자동화 도구입니다.<br>
Gradle 빌드 스크립트들은 Groovy 혹은 Kotlin DSL로 작성할 수 있습니다.
## Gradle로 프로젝트 시작하기
Gradle과 JDK를 모두 설치하셨다면 Gradle의 init명령어로 프로젝트를 시작할 수 있습니다.<br>
먼저 빈 폴더를 하나 만들고 폴더에서 gradle init명령어를 실행합니다.<br>
명령어를 실행하면 무엇을 만들 것인지, 어떤 언어로 만들 것인지,<br>
빌드 스크립트는 무엇으로 만들 것인지, 테스트 프레임워크는 무엇으로 만들 것인지 물어봅니다.<br>
선택해서 완료하면 폴더 내에 다음과 같이 폴더 구조가 생긴 것을 확인할 수 있습니다.<br>
gradle폴더는 Gradle warpper를 위한 파일들이 모여있습니다.<br>
gradlew는 Gradle wrapper 스크립트를 실행할 수 있는 파일입니다. 우리가 애플리케이션을 실행하거나 빌드, 테스트 등을 실행할 때 이 파일로 실행합니다.<br>
settings.gradle파일은 프로젝트 설정 파일입니다. 프로젝트의 이름과 여러 프로젝트들이 있을 때 설정하는 파일입니다.<br>
build.gradle은 애플리케이션 빌드 스크립트 파일인데 의존성 관리도 이 파일에서 합니다.<br>
app 폴더에 src파일에는 우리가 실제로 작성할 파일들이 위치하는 곳이고, test는 테스트 파일들이 모여있는 폴더입니다.<br>
cmd에서 test와 main이 있는 Gradle로 자바프로젝트를 만들 수 있는데<br>
gradle init을 입력하면 무엇을 만들지 선택하는 창이 뜹니다.<br>
2번 application을 눌러 웹 프로젝트를 생성합니다.<br>
언어는 3번 자바를 선택하고, subProject는 1번 no를 입력합니다.<br>
build scropt DSL은 전통적인 1번 그루비를 선택합니다.<br>
테스트 프레임워크는 4번 JUNIT5(Jupiter)를 선택합니다.<br>
그런 다음 Project name과 Source package를 정하면<br>
Gradle로 자바프로젝트가 생성됩니다.<br>
