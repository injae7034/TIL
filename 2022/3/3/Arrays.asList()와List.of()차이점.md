# Arrays.asList()와 List.of()의 차이점

<a href="https://kim-jong-hyun.tistory.com/31" target="_blank">Arrays.asList()와 List.of()의 차이</a>를 참고하였습니다.<br><br>

Arrays.asList() 생성한 List의 객체는 원소 삽입(add), 삭제(remove)가 불가능하지만<br><br>
원소 변경(set, replace)은 가능하고, 원소에 null이 있을 수 있습니다.<br><br>
반면에 List.of()으로 생성한 List의 객체는 원소 삽입(add), 삭제(remove)가 불가능하고<br><br>
원소 변경(set, replace)도 불가능하고, 원소에 null이 있을 수 없습니다.
