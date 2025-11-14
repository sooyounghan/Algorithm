-----
### 문제 28 - 색종이 붙이기 (문제 17136번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/ff107406-7252-4579-936c-bdbd23b1ad6f">
</div>

2. 입력 : 종이의 각 칸에 적힌 수는 총 10줄에 걸쳐 줄 마다 10개의 수로 주어짐
3. 출력 : 모든 1을 덮는 데 필요한 색종이의 최소 개수를 출력하며, 1을 모두 덮지 못할 경우에는 -1을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/836604b2-c070-48ba-b462-3a658c572fd5">
</div>

4. 1단계 : 문제 분석하기
   - 종이의 크기가 10 X 10이고 색종이의 개수가 25개로 많지 않으므로, 색종이를 붙일 수 있는 경우의 수를 모두 탐색
   - 이 때, 모든 경우의 수는 백트래킹 알고리즘을 이용하여 구하면 됨
     + 가능한 선택지 탐색 : 탐색은 색종이를 붙일 수 있는 1번째 위치를 찾는 것
       * 탐색 위치는 왼쪽 위에서 시작하여 오른쪽으로 이동하고, 오른쪽 끝에 도달하면 한 줄 아래로 내려가서 다시 왼쪽부터 이동하는 방식으로 진행 (일관된 방향으로 탐색)
     + 유효성 검사 및 가지치기 : 현재 선택한 위치에 가지고 있는 색종이를 붙일 수 있는지 확인
       * 사용할 수 있는 색종이의 크기는 다섯 종류
       * 크기별로 각각 최대 5장만 사용할 수 있음
       * 색종이를 해당 위치에 붙일 수 있다면 탐색을 계속 진행
       * 붙일 수 없다면 해당 탐색을 종료
       * 목표는 사용한 색종이 개수를 최소화하는 것이므로, 현재까지 사용한 색종이 수가 이미 구한 최솟값을 초과하면 탐색 중단
     + 해답 도출 : 모든 1을 덮는 탐색을 완료하면, 그 과정에서 사용한 색종이 수를 기록
       * 탐색을 모두 끝낸 뒤에는 이렇게 기록된 색종이 수 중에서 가장 작은 값을 찾아 출력

5. 2단계
   - 다음과 같이 크기가 4 X 4인 종이가 있다고 가정하고, 1을 모두 덮는 최소 색종이 수를 도출
   - 코드에서는 색종이를 1로 덮는 과정을 해당 영역의 데이터를 0으로 변경하는 것으로 처리
<div align="center">
<img src="https://github.com/user-attachments/assets/b7c262be-7f7e-4ee4-9be4-6afacfbd3ddd">
</div>

   - 이렇게 해당 범위를 모두 0으로 변경하는 이유는, 이후 탐색 과정에서 같은 영역을 중복하여 색종이로 덮는 실수를 방지하기 위함
   - 백트래킹 알고리즘을 이용해 색종이를 붙일 수 있는 모든 경우의 수를 탐색하고, 그 중 색종이 수가 최소인 경우를 찾아냄
<div align="center">
<img src="https://github.com/user-attachments/assets/3a8f258e-e697-4807-ad23-2deacc83189b">
</div>

   - 그리디 방식으로 먼저 큰 색종이를 덮는 것이 유리해보일 수 있음
     + 하지만 큰 색종이를 먼저 사용했을 때, 오히려 반대로 더 많은 색종이가 필요할 수 있음
     + 따라서, 모든 색종이 크기를 대상으로 탐색하는 과정을 반드시 거쳐야 함
   - 그렇지만 일반적으로 큰 색종이를 먼저 사용하는 것이 더 효율적이므로 코드에서는 큰 색 종이를 사용하면 먼저 탐색하도록 구현
   - 큰 색종이부터 붙이면 적은 수의 색종이 수가 이미 최솟값을 초과하면 즉시 탐색을 종료하는 가지치기로 시간 단축 가능

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/63024967-b4b1-4251-b411-8e31d0af943f">
</div>

7. 코드
```java
package search.backtracking;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class GluingColoredPaper {
    public static int[][] M = new int[10][10];
    public static int[] S = {0, 5, 5, 5, 5, 5}; // 남은 색종이 수
    public static int result = Integer.MAX_VALUE; // 최소로 사용한 개수

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        for(int i = 0; i < 10; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());

            for(int j = 0; j < 10; j++) {
                M[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        // 1이 적힌 모든 칸을 붙일 때 사용한 색종이 개수에 대한 경우의 수를 백트래킹으로 탐색
        backtracking(0, 0);

        if(result == Integer.MAX_VALUE) {
            System.out.println(-1);
        } else {
            System.out.println(result);
        }
    }

    public static void backtracking(int xy, int useCnt) {
        // 색종이로 1이 적힌 모든 칸을 붙였을 때(x, y 좌표 끝까지 탐색한 경우) 탐색 종료
        if(xy == 100) {
            result = Math.min(useCnt, result);
            return;
        }

        int x = xy % 10;
        int y = xy / 10;

        // 가지치기 : 이전에 최소로 사용한 색종이 수보다 현재 탐색에서 사용한 색종이 수가 많으면 탐색 중단
        if(result <= useCnt) {
            return;
        }

        if(M[y][x] == 1) {
            for(int i = 5; i > 0; i--) {
                if(S[i] > 0 && check(x, y, i)) {
                    S[i]--; // 종이 사용하기
                    fill(x, y, i, 0); // 종이 붙이기 : 종이로 덮이는 부분을 1 → 0으로 변경
                    backtracking(xy + 1, useCnt + 1);
                    S[i]++; // 사용한 종이 다시 채우기
                    fill(x, y, i, 1); // 종이 뗴어 내기 : 기존에 덮인 부분을 0 → 1으로 변경
                }
            }
        } else {
            backtracking(xy + 1, useCnt); // 현재 좌표의 값이 0이면 바로 다음 칸으로 이동
        }
    }

    public static void fill(int x, int y, int size, int num) {
        for(int i = y; i < y + size; i++) {
            for(int j = x; j < x + size; j++) {
                M[i][j] = num;
            }
        }
    }

    public static boolean check(int x, int y, int size) {
        if(x + size > 10 || y + size > 10) {
            return false;
        }

        for(int i = y; i < y + size; i++) {
            for(int j = x; j < x + size; j++) {
                if(M[i][j] != 1) return false;
            }
        }
        return true;
    }
}
```
