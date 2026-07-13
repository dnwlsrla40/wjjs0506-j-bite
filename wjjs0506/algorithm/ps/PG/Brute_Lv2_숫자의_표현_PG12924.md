# 숫자의 표현

## 문제정보
- 사이트 : PG(프로그래머스 알고리즘 사이트)
- 문제 번호 : 12924
- 문제 분류 : Brute(Sliding Window)
- 난이도 : Lv2
## 문제 풀이
### 생각한 문제 조건
1. 자료형 제한 조건  
- n이 최대 10000이고, for문의 연산 역시 단순 +연산 이므로 이중 for문(1~n, i~n)으로해도 최대 10000^2이다 (break문도 있기에 더 적게 듬, $n\sqrt{n}$)

>**💬 생각 노트**  
> 나는 단순 brute로 생각하여 문제를 해결하였다.
> 1차적으론 1부터 n까지 돌면서 연속되 수의 합이 n보다 크면 멈추고 n과 같은 순간이 오면 정답을 카운트하는 방식으로 진행하였다.

### 내가 짠 코드
```java
public class PG12924 {
    public static void main(String[] args) {
        System.out.println(solution(15));
    }

    public static int solution(int n) {
        int answer = 0;

        for (int i = 1; i <= n; i++) {
            int count = 0;
            for (int j = i; count + j <= n; j++) {
                count += j;
                if(count == n) {
                    answer++;
                    break;
                }
            }
        }
        return answer;
    }
}

```
## 의문점 및 개선 한 코드

### Sliding Window

연속된 숫자의 합이므로 해당 문제는 sliding window로도 풀수 있다.
기존 방법의 처음부터 누적 합을 계산하는 방법에서 이전의 합을 유지하며, **합을 버리거나 추가하는** 방법을 사용하면 더 효율적이게 해를 구할 수 있다.

```java
public class PG12924 {
    public static void main(String[] args) {
        System.out.println(solution(15));
    }

    public static int solution(int n) {
        int answer = 0;
        int start = 1;
        int end = 1;
        int sum = 1; // 초기 구간 [1, 1]의 합은 1

        while (start <= n) {
            if (sum < n) {
                end++;
                sum += end;
            } else if (sum > n) {
                sum -= start;
                start++;
            } else { // sum == n 인 경우
                answer++;
                sum -= start;   // 기록 후 다음 칸 전진
                start++;
            }
        }
        return answer;
    }
}
```

## 참고