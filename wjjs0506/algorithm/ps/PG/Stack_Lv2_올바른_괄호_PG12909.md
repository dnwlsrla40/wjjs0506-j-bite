# 올바른 괄호 
## 문제정보
- 사이트 : PG(프로그래머스)
- 문제 번호 : 12909
- 문제 분류 : stack
- 난이도 : level 2
## 문제 풀이
### 생각한 문제 조건
1. 기본 조건
   - 열린괄호("(")는 push 닫힌 괄호는(")") pop을 한다.

2. 예외 조건
   - 열린괄호가 없을 때 닫힌 괄호 -> 올바른 괄호 x 이므로 바로 false
   - 문자열 끝난 후에 여전히 열린괄호가 남아있으면 -> 올바른 괄호 x 이므로 바로 false
3. 문제의 다른 조건
     
>**💬 생각 노트**  
> 전형적인 Stack 문제로 "(" 괄호가 먼저 들어가야하므로 해당 괄호는 psuh, ")" 괄호는 pop으로 진행한다. "("이 없을 때 ")"이 나오거나, 모든 입출력 이후에 Stack이 비어있지 않다면 올바른 괄호가 아니라 false를 출력한다.
### 내가 짠 코드
```java
import java.util.Stack;

public class PG12909 {
    public static void main(String[] args) {
        String s = "()()";
        System.out.println(solution(s));
    }

    public static boolean solution(String s) {
        boolean answer = true;
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            if(s.charAt(i) == '(') stack.push('(');
            else if(s.charAt(i) == ')') {
                if(stack.isEmpty()) {
                    answer = false;
                    break;
                }
                stack.pop();
            }
        }
        if(!stack.isEmpty()) answer = false;
        return answer;
    }
}
```
## 의문점 및 개선 한 코드

1. "스택(Stack)을 안 쓰고 풀 수 없나?" 
   
Stack는 자바 극초기(JDK 1.0)에 만들어진 오래된 클래스로 내부적으로 동기화(synchronized) 처리가 되어 있어서, 멀티스레드 환경이 아닐 때도 불필요한 락(Lock)을 걸어 성능이 조금 깎인다.

이 문제는 괄호의 종류가 "(" 하나뿐이기 때문에, 비싼 스택 객체를 생성할 필요 없이 **정수형 카운트 변수 하나(int count = 0)**만으로도 충분하다.

**🛠️ '메모리 다이어트' 코드**

Stack 대신 `int count`를 쓰면 메모리 오버헤드가 거의 제로에 가까워짐

```
public static boolean solution(String s) {
    int count = 0; // 스택의 크기를 대신할 변수

    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) == '(') {
            count++;
        } else {
            if (count == 0) {
                return false; 
            }
            count--;
        }
    }

    // 끝났을 때 count가 0이어야 깔끔하게 짝이 맞은 것
    return count == 0; 
}
```