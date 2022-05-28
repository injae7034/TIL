# 28일에 공부한 내용을 적었습니다.
우아한 스터디를 하였습니다.  

드디어 오늘 스터디에서 책을 다 완독하였습니다.  

책을 다 완독해서 뿌듯했습니다.  

이스트소프트 알약 개발자(경력)로 지원 제안이 와서 코딩테스트를 쳤습니다.  

코딩테스트는 항상 너무 어려운 것 같습니다ㅠ


```java
package injae.AddressBook.common;

import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import static org.assertj.core.api.Assertions.assertThat;

public class codingTest {

    @Test
    void testOne() {
        int[][] histogram = {
                {0,0,0,0,0,0,1},
                {0,0,0,1,0,0,1},
                {0,1,0,1,0,0,1},
                {1,1,2,2,1,0,1},
                {2,2,2,2,1,2,2},
                {2,2,1,1,1,2,2},
                {2,2,1,1,1,2,2}
        };

        int answer = 12;



        List<List<Integer>> sticks = new ArrayList<>();

        int length = histogram[0].length;

        for(int i = 0; i < length; i++){
            List<Integer> stick = new ArrayList<>();

            for(int j = 0; j < histogram.length; j++) {
                stick.add(histogram[j][i]);
            }

            sticks.add(stick);
        }

        List<Integer> cases = new ArrayList<>();

        for (int i = 0; i < histogram.length; i++) {
            cases.add(1);
        }

        int caseIndex = 0;

        for (List<Integer> stick : sticks) {
            for (int i = 0; i < stick.size() - 1; i++) {
                if(stick.get(i) == 1 &&
                        (stick.get(i + 1) == 1 || stick.get(i + 1) == 2)) {
                    cases.set(caseIndex, 1);
                    break;
                }

                if(stick.get(i) == 2 && stick.get(i + 1) == 1) {
                    cases.set(caseIndex, cases.get(i) + 1);
                    break;
                }

                if(stick.get(i) == 2 && stick.get(i + 1) == 2) {
                    cases.set(caseIndex, cases.get(i) + 2);
                }
            }

            caseIndex++;
        }

        int result = 1;
        for (int i = 0; i < cases.size(); i++) {
            result *= cases.get(i);
        }

        assertThat(result).isEqualTo(answer);
//        List<Integer> list = Arrays.asList(0, 0, 0, 1, 2, 2, 2);
//        assertThat(sticks.get(0).toString()).isEqualTo(list.toString());

    }

    @Test
    void testTwo() {
        int[][] histogram = {
                {0,0,0,0,0},
                {0,0,0,0,0},
                {2,2,0,0,0},
                {1,0,1,0,0},
                {2,1,2,2,2},
                {2,1,2,2,2}
        };

        int answer = 18;



        List<List<Integer>> sticks = new ArrayList<>();

        int length = histogram[0].length;

        for(int i = 0; i < length; i++){
            List<Integer> stick = new ArrayList<>();

            for(int j = 0; j < histogram.length; j++) {
                stick.add(histogram[j][i]);
            }

            sticks.add(stick);
        }

//        List<Integer> list = Arrays.asList(0,0,0,1,2,2);
//        assertThat(sticks.get(2).toString()).isEqualTo(list.toString());

        List<Integer> cases = new ArrayList<>();

        for (int i = 0; i < histogram.length; i++) {
            cases.add(1);
        }

        int caseIndex = 0;

        for (List<Integer> stick : sticks) {
            for (int i = 0; i < stick.size() - 1; i++) {
                if(stick.get(i) == 1 &&
                        (stick.get(i + 1) == 1 || stick.get(i + 1) == 2)) {
                    cases.set(caseIndex, 1);
                    break;
                }

                if(stick.get(i) == 2 && stick.get(i + 1) == 1) {
                    cases.set(caseIndex, cases.get(i) + 1);
                    break;
                }

                if(stick.get(i) == 2 && stick.get(i + 1) == 2) {
                    cases.set(caseIndex, cases.get(i) + 2);
                }
            }

            caseIndex++;
        }

        int result = 1;
        for (int i = 0; i < cases.size(); i++) {
            result *= cases.get(i);
        }

//        assertThat(result).isEqualTo(answer);
    }
}

```
