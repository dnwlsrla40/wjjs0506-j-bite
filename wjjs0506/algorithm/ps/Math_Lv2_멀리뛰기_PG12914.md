# 멀리 뛰기
## 문제정보
- 사이트 : PG(프로그래머스 알고리즘 사이트)
- 문제 번호 : 12914
- 문제 분류 : 피보나치 수열
- 난이도 : level 2
## 문제 풀이
### 생각한 문제 조건
1. 자료형 제한 조건  
- 1<= n <= 2000
  
>**💬 생각 노트**  
> 처음엔 dfs 형식으로 1로 방문하는 경우와 2로 방문하는 경우를 계산하여서 계획하였지만, 시간 초과가 발생하여 결과값이 피보나치 수열의 형식을 이루는 규칙을 발견해 진행하였다.
> 그러나 해당 방법 역시 재귀 형식으로 구현하여 소요시간이 오래 걸려 최종적으로 배열형식으로 구현하여 통과하였다.


### 내가 짠 코드
```java
public class PG12914 {
    private static long count = 0;

    public static void main(String[] args) {
        System.out.println(solution(4));
    }

    public static long solution(int n) {
        dfs(n, 0);
        return count % 1234567;
    }

    public static void dfs(int n, int depth) {
        // 1. 탈출 조건
        if(depth == n) {
            count++;
            return;
        }

        if(depth > n) {
            return;
        }

        dfs(n, depth+1);
        dfs(n, depth+2);
    }
}

```

## 의문점 및 개선 한 코드


### 재귀 형의 fibo 나치 구현

1과 2일 때는 바로 return 해주고 그 상의 수일 경우, (n-1 + n-2) 값을 return 했지만, 재귀 형식이라 여전히 시간초과가 발생하였다.

```JAVA
public static int fibo(int n) {
    if( n == 1) {
        return 1;
    } else if(n == 2) {
        return 2;
    }

    return fibo(n-1) + fibo(n-2);
}
```

배열의 형식으로 이전의 값을 저장해두어 가져다 쓸 수 있게 변경하였다.
```java
class Solution {    
    public static long solution(int n) {
        int[] arr = new int[n+1];

        if(n == 0) return 0;
        else if(n == 1) return 1 % 1234567;
        else if(n == 2) return 2 % 1234567;

        arr[1] = 1 % 1234567;
        arr[2] = 2 % 1234567;

        for (int i = 3; i <= n; i++) {
            arr[i] = (arr[i-1] + arr[i-2]) % 1234567;
        }
        return arr[n];
    }
}
```