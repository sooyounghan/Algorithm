-----
### 문제 29 - DFS와 BFS 프로그램 (문제 1260번)
-----
1. 문제
```
그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오.
단, 방문할 수 있는 노드가 여러 개일 경우 노드 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 노드가 없을 떄 종료한다.
노드 번호는 1에서 N까지다.
```

2. 입력
   - 1번째 줄에 노드의 개수 N(1 ≤ N ≤ 1,000), 엣지의 개수 M(1 ≤ M ≤ 10,O00), 탐색을 시작할 노드 번호 V가 주어짐
   - 그 다음 M개의 줄에는 엣지가 연결하는 두 노드의 번호가 주어짐
   - 어떤 두 노드 사이에 여러 개 엣지가 있을 수 있으며, 입력으로 주어지는 엣지는 양방향

3. 출력
   - 1번쨰 줄에 DFS를 수행한 결과, 그 다음 줄에 BFS를 수행한 결과를 출력
   - V부터 방문된 점을 순서대로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/c7a3b320-dccb-485f-b476-92a64fb6b049">
</div>


4. 1단계 : 문제 분석하기
   - DFS와 BFS를 구현할 수 있는지 물어보는 기본 문제

5. 2단계
   - 인접 리스트에 그래프를 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/edb20a57-70d4-4ab0-9ac9-fd635eff60f3">
</div>

   - DFS를 실행하면서 방문 배열 체크와 탐색 노드 기록을 수행
     + 문제 조건에서 작은 번호의 노드부터 탐색한다고 했으므로 인접 노드를 오름차순으로 정렬한 후 재귀 함수를 호출
<div align="center">
<img src="https://github.com/user-attachments/assets/3f8564c9-5877-455e-ba85-55974ca41c92">
</div>

   - 1을 방문 배열에 체크하고 1을 pop하며 탐색 경로 기록을 진행
     + 인접 노드는 2, 3, 4
     + 작은 번호부터 탐색해야 하므로 2를 재귀함수로 호출
   - 2를 방문 배열에 체크하고 다시 pop하며 인접 노드를 봄
     + 인접 노드는 4, 1
     + 1, 4로 정렬하여 재귀 함수로 호출하되 1은 방문했으므로 4를 다시 재귀 함수로 호출

   - BFS도 같은 방식으로 진행 : 노드를 오름차순으로 정렬하여 큐에 삽입
<div align="center">
<img src="https://github.com/user-attachments/assets/d51cd342-7714-4e4c-bdd5-4f691f83a325">
</div>

   - DFS와 BFS를 마치면, 각각 탐색하며 기록한 데이터 출력

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/08597522-f80f-4c1c-b63f-fde095295491">
</div>

7. 코드
```java
package search.bfs;

import java.util.*;

public class DFSandBFS {
    public static boolean visited[];
    public static ArrayList<Integer>[] A;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt(); // 노드 개수
        int M = sc.nextInt(); // 엣지 개수
        int start = sc.nextInt();

        A = new ArrayList[N + 1];

        for (int i = 1; i <= N; i++) {
            A[i] = new ArrayList<Integer>();
        }

        for (int i = 0; i < M; i++) {
            int S = sc.nextInt();
            int E = sc.nextInt();

            A[S].add(E);
            A[E].add(S);
        }

        // 번호가 작은 것부터 먼저 방문하기 위해 정렬
        for(int i = 1; i <= N; i++) {
            Collections.sort(A[i]);
        }

        visited = new boolean[N + 1]; // 방문 배열 초기화
        DFS(start);
        System.out.println();
        visited = new boolean[N + 1]; // 방문 배열 초기화
        BFS(start);
        System.out.println();
    }

    public static void DFS(int Node) { // DFS 구현
        System.out.print(Node + " ");

        visited[Node] = true;

        for (int i : A[Node]) {
            if(!visited[i]) {
                DFS(i);
            }
        }
    }

    public static void BFS(int Node) {
        Queue<Integer> queue = new LinkedList<Integer>();
        queue.add(Node);
        visited[Node] = true;

        while(!queue.isEmpty()) {
            int nowNode = queue.poll();
            System.out.print(nowNode + " ");

            for (int i : A[nowNode]) {
                if(!visited[i]) {
                    visited[i] = true;
                    queue.add(i);
                }
            }
        }
    }
}
```
