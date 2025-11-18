-----
### 문제 98 - 외판원의 순회 경로 짜기 (문제 2098번)
-----
1. 문제
```
1번부터 N번까지 번호가 매겨져 있는 도시들이 있고, 도시들 사이에는 길이 있다.(길이 없을 수도 있다.)

이제 한 외판원이 어느 한 도시에서 출발해 N개의 도시를 모두 거쳐 다시 원래의 도시로 돌아오는 순회 여행 경로를 계획하려고 한다.
단, 한 번 갔던 도시로는 다시 갈 수 없다. (맨 마지막에 여행을 출발했던 도시로 돌아오는 것은 예외)

이런 여행 경로에는 여러 가지가 있을 수 있는데, 가장 적은 비용을 들이는 여행 계획을 세우고자 한다.

각 도시 간에 이동하는 데 드는 비용은 행렬 W[i][j] 형태로 주어진다.
W[i][j]는 도시 i에서 도시 j로 가기 위한 비용을 나타낸다. 비용은 대칭적이지 않다.
즉, W[i][j]는 W[j][i]와 다를 수 있다.

모든 도시 간의 비용은 양의 정수다. W[i][i]는 항상 0이다.
경우에 따라 도시 i에서 도시 j로 갈 수 없을 때도 있고, 이럴 경우 W[i][j] = 0이라고 가정한다.

N과 비용 행렬이 주어졌을 때, 가장 적은 비용을 들이는 외판원의 순회 여행 경로를 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 도시의 수 N이 주어짐 (2 ≤ N ≤ 16)
   - 다음 N개의 줄에는 비용 행렬이 주어짐 : 각 행렬의 성분은 1,000,000 이하의 양의 정수이며, 갈 수 없을 때는 0이 주어짐
   - W[i][j]는 도시 i에서 j로 가기 위한 비용을 나타냄
   - 항상 순회할 수 있을 때만 입력으로 주어짐

3. 출력 : 1번째 줄에 외판원의 순회에 필요한 최소 비용을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/6e91b292-ccc1-4734-9492-4eddeaa9069c">
</div>

4. 1단계 : 문제 분석하기
   - 외판원 순회 문제는 영어로 TSP(Traveling Salesman Problem)이라 불리고, 컴퓨터 과학 분야에서 가장 중요하게 취급되는 문제 중 하나
   - N의 크기가 매우 작으므로, 모든 순서를 완전 탐색으로 수행하면 정답을 구할 수 있음
   - 점화식 정의
<div align="center">
<img src="https://github.com/user-attachments/assets/2fe0afe1-9888-4724-a035-74172d0c0032">
</div>

   - 예를 들어, D[2][1, 2]는 현재 도시가 2이고, 1, 2 도시를 방문한 상태에서 나머지 모든 도시를 경유하는 데 필요한 비용을 의미
   - 완전 탐맥의 경우에는 DFS, BFS와 같지만, 생각해야 할 문제
     + D[i][j]에서 j가 나타내는 것이 현재까지 방문한 모든 도시 리스트
     + 리스트 데이터를 j라는 변수 1개에 저장할 수 있는 방법 : 비트(Bit), 즉 2진수로 표현

   - 총 도시가 4개일 때를 예로 설정할 때, 방문 도시를 이진수로 표현 : 방문 도시를 이진수의 각 자릿수로 표현하고, 방문 시 1, 미방문 시 0의 값으로 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/5cc510ab-646e-434c-a20c-829abc3a3347">
</div>

   - 이러한 방식으로 방문 리스트를 1개의 변수로 표현할 수 있음

5. 2단계
   - 점화식 : c번 도시에서 v 리스트 도시를 방문한 후 남은 모든 도시를 순회하기 위한 최소 비용을 구하려면, 현재 방문하지 않은 모든 도시에 대해 다음 도시로 선택하고 점화식을 이용해 비용을 갱신하는 과정 반복
     + 방문하지 않은 도시를 i라고 했을 때, W[c][i]는 도시 c에서 도시 i로 가기 위한 비용
<div align="center">
<img src="https://github.com/user-attachments/assets/083e3897-f2f0-4a62-8d9d-ee17ded96c24">
</div>

 3. 점화식에 사용한 비트 연산식
<div align="center">
<img src="https://github.com/user-attachments/assets/37602637-2353-41d8-a8ac-b14554456c2b">
</div>

   - 15를 이진수로 표현하면 1111으로 표현되며, 이진수의 각 자리가 모두 1이므로 모든 도시를 방문한 상태 : 즉, 도시의 개수가 4개인데 v값이 15인 경우 모든 도시를 방문한 상태
<div align="center">
<img src="https://github.com/user-attachments/assets/63a0a1cb-44c1-4eba-861d-e0b92893b347">
</div>

   - v & 1000연산을 수행했을 때 결과값이 0이면 4번 도시를 방문하지 않았다고 판단 : 즉, v의 이진수 표현 시 4번째 자리가 1인 아닌 경우가 아니면 0을 반환하며, 4번쨰 도시를 방문하지 않았다고 판단
<div align="center">
<img src="https://github.com/user-attachments/assets/f80f9764-2142-4d50-b203-9975e570bf25">
</div>

   - v | 100 연산을 수행하면 v의 이진수 표현 시 3번쨰 자리를 1로 저장하게 됨 : 3번째 도시를 방문하였다는 사실을 저장

6. 2단계 - 계속
   - W 배열을 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/0a5e39ae-ca4c-42f4-bc0e-6d2d047d1394">
</div>

   - 점화식으로 정답을 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/f42feab2-53f3-4d6e-9648-b915527221f0">
</div>

   - 최솟값으로 정답을 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/ef36c022-d38f-4f3d-9168-efb81114a388">
</div>

7. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/e9119682-0f44-4659-8f4c-69013320bfac">
</div>

8. 코드
```java
package dynamicprogramming;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class TSP {
    private static final int INF = 1000000 * 16 + 1;
    private static int N;
    private static int[][] W;
    private static int[][] d;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = null;

        N = Integer.parseInt(br.readLine().trim());
        W = new int[16][16];
        d = new int[16][1 << 16];

        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine().trim());

            for (int j = 0; j < N; j++) {
                W[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        System.out.println(tsp(0, 1));
    }

    private static int tsp(int c, int v) { // 모든 경우의 수와 관련해 완전 탐색과 재귀 구현
        if (v == (1 << N) - 1) { // 모든 노드를 방문할 때
            return W[c][0] == 0 ? INF : W[c][0];
        }

        if (d[c][v] != 0) { // 이미 방문한 노드일 때 : 바로 반환 (메모이제이션)
            return d[c][v];
        }

        int min_val = INF;

        for (int i = 0; i < N; i++) {
            if((v & (1 << i)) == 0 && W[c][i] != 0) { // 방문한 적이 없고, 갈 수 있는 도시일 때
                min_val = Math.min(min_val, tsp(i, (v | (1 << i))) + W[c][i]);
            }
        }

        d[c][v] = min_val;
        return d[c][v];
    }
}
```
