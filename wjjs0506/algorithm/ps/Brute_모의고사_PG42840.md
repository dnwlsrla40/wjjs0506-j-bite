# 모의고사
## 문제정보
- 사이트 : PG(프로그래머스)
- 문제 번호 : 42840
- 문제 분류 : Brute
- 난이도 : level 1
## 문제 풀이
     
>**💬 생각 노트**  
> 특정한 반복 패턴으로 정답을 찍으므로, 각 패턴의 크기로 나눈 나머지인 i번째와 패턴의 i번째가 같은 지 확인하여 정답 수를 count한다.


### 내가 짠 코드
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class PG42840 {

    static int[] arr1 = {1,2,3,4,5};
    static int[] arr2 = {2,1,2,3,2,4,2,5};
    static int[] arr3 = {3,3,1,1,2,2,4,4,5,5};
    
    public static  int[] solution(int[] answers) {
        int[] answer = {};
        List<Integer> list = new ArrayList<>();
        int count1 = 0,count2 = 0,count3 = 0;
        for (int i = 0; i < answers.length; i++) {
            if (answers[i] == arr1[i % 5]) count1++;
            if (answers[i] == arr2[i % 8]) count2++;
            if (answers[i] == arr3[i % 10]) count3++;
        }

        int maxScore = Math.max(count1, Math.max(count2, count3));
        if (count1 == maxScore) list.add(1);
        if (count2 == maxScore) list.add(2);
        if (count3 == maxScore) list.add(3);

        answer = list.stream().mapToInt(i -> i).toArray();
        return answer;
    }
}
```
## 의문점 및 개선 한 코드


## 참고