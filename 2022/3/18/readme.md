# 18일에 공부한 내용을 적었습니다.
## 문자열 덧셈 계산기 재구현
어제 구현한 문자열 덧셈계산기에서 StringCalculator가 문자열 분리까지 하고 있어서<br><br>
하는 일이 너무 많아 이를 분리하기 위해 NubmersParser클래스를 생성하여<br><br>
문자열에서 숫자를 분리하는 역할을 넘겼습니다.<br><br>
이로 인해 StringCalculator는 이제 분리된 숫자문자열들을 덧셈만 해주는 역할을 하게 되었습니다.<br><br>
또한 NubmersParser클래스를 테스트하기 위한 코드도 추가했습니다.<br><br>

## 자동차 경주 게임 구현하기
일단은 제시한 사항대로 돌아갈 수 있는 프로그램을 구현하였으나,<br><br>
UI영역과 도메인 영역 구분이 좀 모호한 점도 있고,<br><br>
램덤으로 숫자를 고르는 영역에서 TDD를 하지 못한 점도 있습니다.<br><br>
이를 보완하여 다시 TDD를 기반으로 재구현해야하고,<br><br>
재구현할 때, 일급컬렉션과 원시자료형 포장하는 것도 시도해야겠습니다.