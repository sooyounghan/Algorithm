-----
### 문제 23 - 연결 요소의 개수 (문제 11724번)
-----
1. 문제
```
방향 없는 그래프가 주어졌을 때 연결 요소(Connected Component)의 개수를 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 노드의 개수 N (1 ≤ N ≤ 1,000)과 엣지의 개수 M(0 ≤ M ≤ N X (N - 1) / 2)
   - 2번째 줄부터 M개의 줄에 엣지의 양끝 점 u와 v가 주어짐 (1 ≤ u, v ≤ N, u ≠ v)
   - 같은 엣지는 한 번만 주어짐

3. 출력 : 1번째 줄에 연결 요소의 개수를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/0ec14487-51dc-43da-a69e-2153f5ac2aeb">
</div>

4. 1단계 : 문제 분석하기
   - 노드의 최대 개수가 1,000이므로 시간 복잡도 O($N^{2}$)이하의 알고리즘을 모두 사용 가능
   - 연결 요소는 엣지로 연결된 노드의 집합이며, 한 번의 DFS가 끝날 때까지 모든 노드의 집합을 하나의 연결 요소로 판단 가능

5. 2단계
   - 그래프를 인접 리스트로 저장하고 방문 배열도 초기화 : 방향이 없는 그래프이므로 양쪽 방향으로 엣지 모두 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/f0df53b1-69be-49d6-920e-6089071c93eb">
</div>

   - 임의의 싲가점에서 DFS 수행 : 현재의 경우 1을 시작점으로 지정
   - 탐색을 마친 이후 방문한 곳은 1, 2, 5
<div align="center">
<img src="https://github.com/user-attachments/assets/ad7e8264-8f62-4508-883c-7a16ffa3dc70">
</div>

   - 아직 방문하지 않은 노드가 있으므로, 시작점을 다시 정해 탐색을 진행 : 현재의 경우 3, 4, 6 순서로 탐색을 완료
   - 모든 노드를 방문했으므로 전체 탐색 종료
<div align="center">
<img src="https://github.com/user-attachments/assets/204b29ee-2727-4fe8-9abf-fd7ba10fb442">
</div>

   - 1 ~ 3 과정을 통해 총 2번의 DFS가 진행
   - 즉, 연결 요소의 개수는 2개
<div align="center">
<img src="https://github.com/user-attachments/assets/aa1c8e8e-741f-4855-9942-61b611d08837">
</div>

   - 만일 그래프가 모두 연결되었다면 DFS는 1번만 실행
   - 다시 말해, 모든 노드를 탐색하는 데 실행한 DFS 실행 횟수가 곧 연결 개수와 동일

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/61bb7487-f835-450a-9e30-69097ff95c66">
</div>

   - 재귀 함수는 스택과 같은 방식으로 처리되므로, 위의 슈도코드는 연결리스트와 스택을 사용한 방식과 같은 방식으로 구현된 것이라 할 수 있음

7. 코드
```java
package search.dfs;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.StringTokenizer;

public class ConnectedComponents {
    static ArrayList<Integer>[] A;
    static boolean visited[];

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());

        A = new ArrayList[n + 1];
        visited = new boolean[n + 1];

        for(int i = 1; i <= n; i++) { // 인접 리스트 초기화
            A[i] = new ArrayList<Integer>();
        }

        for(int i = 1; i <= m; i++) {
            st = new StringTokenizer(br.readLine());

            int s = Integer.parseInt(st.nextToken());
            int e = Integer.parseInt(st.nextToken());

            A[s].add(e); // 양방향 엣지이므로 양쪽에 엣지 더하기
            A[e].add(s);
        }

        int count = 0;
        for(int i = 1; i < n + 1; i++) {
            if(!visited[i]) { // 방문하지 않은 노드가 없을 때까지 반복
                count++;
                DFS(i);
            }
        }

        System.out.println(count);
    }

    public static void DFS(int v) {
        if(visited[v]) {
            return;
        }

        visited[v] = true;

        for (int i : A[v]) {
            if(visited[i] == false) { // 연결 노드가 방문하지 않았던 노드만 탐색
                DFS(i);
            }
        }
    }
}
```
