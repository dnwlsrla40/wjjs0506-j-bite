# 게임 맵 최단 거리
## 문제정보
- 사이트 : PG(프로그래머스)
- 문제 번호 : 1844
- 문제 분류 : bfs
- 난이도 : Level2

## 문제 풀이

### 생각한 문제 조건
1. 자료형 제한 조건 : 2차원 int 배열로 표현 가능, 방문 여부 visit는 boolean형
2. 용어 정의  
3. 문제의 다른 조건  
> **💬 생각 노트**  
> 기본적인 bfs 문제로 너비우선 탐색으로 최소 값을 가지는 길의 여부를 찾는다.
### 내가 짠 코드
```java
import java.util.LinkedList;
import java.util.Queue;

public class PG1844 {
    public static int[] dx = {0,1,0,-1};
    public static int[] dy = {1,0,-1,0};

    public static void main(String[] args) {
        int[][] maps = {{1,0,1,1,1},{1,0,1,0,1},{1,0,1,1,1},{1,1,1,0,1},{0,0,0,0,1}};
        System.out.println(solution(maps));
    }

    public static int solution(int[][] maps) {
        boolean[][] visit = new boolean[maps.length][maps[0].length];;
        Queue<int[]> queue = new LinkedList<>();

        queue.offer(new int[]{0,0});
        while(!queue.isEmpty()) {
            int[] temp = queue.poll();
            int tempX = temp[0];
            int tempY = temp[1];

            if(tempX == maps.length-1 && tempY == maps[0].length-1) {
                return maps[tempX][tempY];
            }

            for (int i = 0; i < 4; i++) {
                int nx = temp[0] + dx[i];
                int ny = temp[1] + dy[i];
                if(nx < 0 || nx >= maps.length || ny < 0 || ny >= maps[0].length) continue;
                if(visit[nx][ny] || maps[nx][ny] == 0) continue;

                visit[nx][ny] = true;
                maps[nx][ny] = maps[tempX][tempY] + 1;
                queue.offer(new int[]{nx,ny});

            }
        }
        return -1;
    }

}

```
## 의문점 및 개선 한 코드
## 참고