# 최댓값과 최솟값
## 문제정보
- 사이트 : PG(프로그래머스)
- 문제 번호 : 12939
- 문제 분류 : String
- 난이도 : level 2
## 문제 풀이
### 생각한 문제 조건

>**💬 생각 노트**  
> String을 공백 기준으로 나눠 각 숫자를 비교하여 최대, 최솟값을 구한뒤 출력한다.

### 내가 짠 코드
```java
public class PG12939 {

    public static void main(String[] args) {
        String s = "-1 -2 -3 -4";
        System.out.println(solution(s));

    }

    public static String solution(String s) {
        String answer = "";
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        for (String str : s.split(" ")) {
            int num = Integer.parseInt(str);
            max = Math.max(max, num);
            min = Math.min(min, num);
        }
        answer = min + " " + max;
        return answer;
    }
}

```
## 의문점 및 개선 한 코드

