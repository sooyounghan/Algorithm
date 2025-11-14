-----
### 문제 30 - 미로 탐색하기 (문제 2178번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/21175dc2-1d68-4645-b63d-ec1d9dcf4066">
</div>

2. 입력
   - 1번쨰 줄에 두 정수 N, M(2 ≤ N, M ≤ 100)
   - 그 다음 N개의 줄에는 미로의 내용이 M개의 정수로 주어짐
   - 각각의 수들은 붙어서 입력

3. 출력 : 1번째 줄에 지나야 하는 칸 수의 최솟값을 출력 (항상 도착 위치로 이동할 수 있을 때만 입력으로 주어짐)
<div align="center">
<img src="https://github.com/user-attachments/assets/905d83f7-0cc4-4362-9905-d13a18befae5">
</div>

4. 1단계 : 문제 분석하기
   - N, M의 최대 데이터 크기가 100으로 매우 작으므로 시간 제한은 별도로 생각하지 않아도 되는 문제
   - 문제의 요구사항 : 지나야 하는 칸 수의 최솟값 찾기
     + 이는 완전 탐색을 진행하며 몇 번쨰 깊이에서 원하는 값을 찾을 수 있는지 찾을 수 있는지 구하는 것과 동일
   - 따라서, BFS를 사용해 최초로 도달했을 때 깊이를 출력하면 문제 해결 가능
   - DFS보다 BFS가 적합한 이유는 BFS는 해당 깊이에서 갈 수 있는 노드 탐색을 마친 후 다음 깊이로 넘어가기 때문임

5. 2단계
   - 예제 입력 2번을 이용
   - 먼저 2차원 배열에 데이터를 저장한 다음 (1, 1)에서 BFS를 실행
   - 상, 하, 좌, 우 네 방향을 보며 인접한 칸을 봄 : 인접한 칸의 숫자가 1이면서 아직 방문하지 않았다면 큐에 삽입
   - 종료 지점 (N, M)에서 BFS를 종료하며 깊이를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/45cc98c0-897c-4911-a7ed-50c08ac451a9">
</div>

   - 그림을 보면 (1, 1)에서 출발해 상, 하, 좌, 우 순서로 노드를 큐에 삽입하며 방문 배열에 체크
   - 현재의 경우 하, 우만 방문할 수 있으므로, 하, 우 순서로 노드를 큐에 삽입
   - 이런 방식으로 노드를 방문하면 깊이 9단계에서 (4, 6)에 도달

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/d2c202ab-9b76-49f3-bff7-1b2108795971">
</div>

7. 코드
```java
package search.bfs;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class MazeSearch {
    // 상, 하, 좌, 우를 탐색하기 위한 배열 선언
    public static int[] dx = { 0, 1, 0, -1 }; // 우, 하, 좌, 상
    public static int[] dy = { 1, 0, -1, 0 };

    public static boolean[][] visited;
    public static int[][] A;
    public static int N, M;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        A = new int[N][M];
        visited = new boolean[N][M];

        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());

            String line = st.nextToken();

            for (int j = 0; j < M; j++) {
                A[i][j] = Integer.parseInt(line.substring(j, j + 1));
            }
        }

        BFS(0, 0);
        System.out.println(A[N - 1][M - 1]);
    }

    public static void BFS(int i, int j) {
        Queue<int[]> queue = new LinkedList<>();

        queue.offer(new int[] {i, j} );
        visited[i][j] = true;

        while (!queue.isEmpty()) {
            int[] now = queue.poll();

            for (int k = 0; k < 4; k++) {
                int x = now[0] + dx[k];
                int y = now[1] + dy[k];

                if (x >= 0 && y >= 0 && x < N && y < M) { // 좌표 유효성 검사하기
                    if (A[x][y] != 0 && !visited[x][y]) { // 갈수 있는 칸 && 방문 검사하기
                        visited[x][y] = true;
                        A[x][y] = A[now[0]][now[1]] + 1; // 길이 업데이트 하기
                        queue.add(new int[] {x, y});
                    }
                }
            }
        }
    }
}
```
