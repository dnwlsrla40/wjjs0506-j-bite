# N개의 최소공배수
## 문제정보
- 사이트 : PG(프로그래머스 알고리즘 사이트)
- 문제 번호 : 12953
- 문제 분류 : 최소공배수
- 난이도 : level 2
## 문제 풀이
### 생각한 문제 조건
1. 자료형 제한 조건  
- 1 <= n <= 15
- arr 원소 <= 100
  
>**💬 생각 노트**  
> N이 15개라 기본적인 최소공배수 구하는 과정으로 접근하여 공통된 배수를 찾아서 진행하였지만, 원소의 값이 100에 근사한 값이 많을 수록 메모리 및 성능적으로 문제가 발생하였다.
> 이후, `최소 공배수 = 두 수의 곱 / 최대공약수`와 유클리드 호제법을 활용하여 문제를 풀었다.


### 내가 짠 코드
```java
public static int solution(int[] arr) {
        int answer = 0;
        int max = 1;

        for (int i = 0; i < arr.length; i++) {
            max *= arr[i];
        }

        int[] result = new int[max+1];
        int index = 0;
        while(index < arr.length){
            for (int i = 1; i*arr[index] <= max; i++) {
                result[i*arr[index]]++;
            }
            index++;
        }

        for (int i = 0; i < result.length; i++) {
            if(result[i] == arr.length){
                answer = i;
                break;
            }
        }
        return answer;
    }

```

## 개선 한 코드

```java
public static int solution(int[] arr) {
    int answer = 0;
    int a = arr[0];
    int b = arr[1];
    answer = lcm(a, b);
    for (int i = 2; i < arr.length; i++) {
        answer = lcm(arr[i], answer);
    }
    return answer;
}

public static int lcm(int a, int b){
    if(a > b) {
        return (a*b)/gcd(a, b);
    } else  {
        return (a*b)/gcd(b, a);
    }
}

public static int gcd(int a, int b){
    if(b==0) {
        return a;
    }
    return gcd(b, a%b);
}
```