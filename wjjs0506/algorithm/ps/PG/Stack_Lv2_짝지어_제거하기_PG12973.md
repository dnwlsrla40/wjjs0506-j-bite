# 짝지어 제거하기
## 문제정보
- 사이트 : PG(프로그래머스)
- 문제 번호 : 12973
- 문제 분류 : Stack
- 난이도 : level 2
## 문제 풀이
### 생각한 문제 조건

>**💬 생각 노트**  
> 쌍으로 만나서 조건 진행하므로, Stack을 사용하고 마지막에 Stack에 데이터가 남아있는 지 확인한다.
> 초기엔 Brute force로 진행하였지만, 효율성 테스트에서 걸려 Stack으로 사용하였다.

### 내가 짠 코드
```java
// 처음의 brute force 방법
public static int solution(String s) {
        while (true) {
            boolean flag = false;
            for (int i = 0; i < s.length(); i++) {
                if(i+1 == s.length()) break;
                if(s.charAt(i) == s.charAt(i+1)){
                    flag = true;
                    s = s.replace(""+s.charAt(i)+s.charAt(i+1), "");
                }
            }
            if(!flag) break;
        }

        return s.isEmpty() ? 1 : 0;
    }
```


```java
// Stack 사용
import java.util.Stack;

public class PG12973 {

    public static void main(String[] args) {
        String s = "cdcd";
        System.out.println(solution(s));
    }

    public static int solution(String s) {
        Stack<Character> stack = new Stack();

        for (Character c : s.toCharArray()) {
            if(stack.isEmpty()){
                stack.push(c);
            } else {
                if(stack.peek().equals(c)){
                    stack.pop();
                } else {
                    stack.push(c);
                }
            }
        }

        return stack.isEmpty() ? 1 : 0;
    }
}
```