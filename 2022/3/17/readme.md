# 17일에 공부한 내용을 적었습니다.
숫자야구게임을 처음부터 안보고 다시 구현하였습니다.<br><br>
enum클래스인 BallStatus에서 ballStatus가 nothing인지 아닌지를 비교할 때,<br><br>
this와 nothing을 비교한 결과를 출력하면 되는데 그것을 까먹어서 뭐라 NOTHING이라 비교해야할지 몰라서 헤맸습니다.<br><br>
Ball 클래스는 똑같이 안보고 구현을 했으나 이번에도 역시 Balls 클래스를 구현할 때 헤맸습니다.<br><br>
Ball의 객체를 매개변수로 입력 받아 BallStatus를 반환하는 play메소드를 구현할 때 스트림을 이용하는데<br><br>
여기서 헤멨습니다.<br><br>
```java
public BallStatus play(Ball other) {
    return balls.stream()
          .map(ball -> ball.play(other))
          .filter(BallStatus::isNotNothing)
          .findFirtst()
          .orElse(BallStatus.NOTHING);
```

또한 이번에도 PlayResult클래스를 생성하는 것을 깜빡하여 play메소드를 구현할 때 헤맸습니다.<br><br>
```java
pulbic PlayResult play(List<Integer> ballNumbers) {
    Balls balls = new Balls(ballNumbers);
    PlayResult playResult = new PlayResult();
    for(Ball ball : this.balls) {
        BallStatus ballStatus = balls.play(ball);
        playResult.report(ballStatus);
    }
    
    return playResult;
}
```
그 이외에는 구현하는데 문제는 없었습니다.<br><br>

# 문자열덧셈계산기 구현하기
TDD를 바탕으로 하여 문자열덧셈계산기를 구현하였습니다.<br><br>
내일 좀 더 수정한 다음 자동차경주게임을 구현할 것입니다.<br><br>
