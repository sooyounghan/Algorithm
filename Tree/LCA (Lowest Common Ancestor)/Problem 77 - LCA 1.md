-----
### 문제 77 - 최소 공통 조상 구하기 1 (문제 11437번)
-----
1. 문제
```
N(2 ≤ N ≤ 50,000)개의 노드로 이루어진 트리가 주어진다.
트리의 각 노드는 1번부터 N번까지 번호가 매겨져 있으며, 루트는 1번이다.

두 노드의 쌍(1 ≤ M ≤ 10,000)개가 주어졌을 때, 두 노드의 가장 가까운 공통 조상이 몇 번인지 출력하시오.
```

2. 입력
   - 1번쨰 줄에 노드의 개수 N이 주어짐
   - 그 다음 N - 1개 줄에는 트리 상에서 연결된 두 노드가 주어짐
   - 그 다음 줄에는 가장 가까운 공통 조상을 알고 싶은 쌍의 개수 M이 주어짐
   - 그 다음 M개의 줄에는 노드 쌍이 주어짐

3. 출력 : M개의 줄에 차례대로 입력받은 두 노드의 가장 가까운 공통 조상 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/c908885c-522d-46f9-b3a2-e56e47b10554">
</div>

4. 1단계 : 문제 분석하기
   - 질의 개수가 10,000개이며, 노드 개수가 50,000개이므로 비교적 데이터가 크지 않아 일반적인 LCA 알고리즘으로 구현할 수 있는 문제

5. 2단계
   - 인접 리스트로 트리 데이터를 구현
   - 탐색 알고리즘(BFS, DFS)를 이용해 각 노드의 깊이를 구함 : 여기서는 LCA(6, 11)에 해당하는 두 노드만 확인
<div align="center">
<img src="https://github.com/user-attachments/assets/71f12171-2f33-4b6f-b608-284583289478">
</div>

   - 깊이를 맞추기 위해 더 깊은 노드를 같은 깊이가 될 때까지 부모 노드로 이동 : 깊이가 1만큼 차이나므로 깊이가 3인 11번 노드를 부모 노드로 한 번 업데이트
<div align="center">
<img src="https://github.com/user-attachments/assets/82d9123b-6410-40a0-ba6f-9885835864d2">
</div>

   - 부모 노드로 계속 올라가면서 최소 공통 조상을 찾음 : 한 번 더 이동하면 부모 노드가 2로 같아지므로 노드 6과 11의 최소 공통 조상은 2번 노드
<div align="center">
<img src="https://github.com/user-attachments/assets/e0d24032-8897-4693-9aac-10711b010c0c">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/2a70dab5-90e9-4cdc-bed1-4ca55298eefc">
</div>

7. 코드
```java
package tree.lca;

import org.w3c.dom.Node;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class LCA {
    public static ArrayList<Integer>[] tree;
    public static int[] depth;
    public static int[] parent;
    public static boolean[] visited;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt(); // 노드의 수
        tree = new ArrayList[N + 1];

        for(int i = 1; i <= N; i++) {
            tree[i] = new ArrayList<Integer>();
        }

        for (int i = 0; i < N - 1; i++) { // 인접 리스트에 그래프 데이터 저장하기
            int s = sc.nextInt();
            int e = sc.nextInt();

            tree[s].add(e);
            tree[e].add(s);
        }

        depth = new int[N + 1];
        parent = new int[N + 1];
        visited = new boolean[N + 1];

        BFS(1); // Depth를 BFS를 이용해 구하기

        int M = sc.nextInt(); // 질의의 수
        for (int i = 0; i < M; i++) {
            // 공통 조상을 구할 두 노드
            int a = sc.nextInt();
            int b = sc.nextInt();

            int LCA = executeLCA(a, b);
            System.out.println(LCA);
        }
    }

    // BFS 구현

    private static void BFS(int node) {
        Queue<Integer> queue = new LinkedList<>();

        queue.add(node);
        visited[node] = true;

        int level = 1;
        int now_size = 1;
        int count = 0;

        while(!queue.isEmpty()) {
            int nowNode = queue.poll();

            for (int next : tree[nowNode]) {
                if (!visited[next]) {
                    visited[next] = true;
                    queue.add(next);

                    parent[next] = nowNode; // 부모 노드 저장
                    depth[next] = level;
                }
            }

            count++;

            if (count == now_size) {
                count = 0;
                now_size = queue.size();
                level++;
            }
        }
    }

    public static int executeLCA(int a, int b) {
        if(depth[a] < depth[b]) {
            int temp = a;
            a = b;
            b = temp;
        }

        while(depth[a] != depth[b]) { // 두 노드의 Depth 맞추기
            a = parent[a];
        }

        while(a != b) { // 같은 조상이 나올 때까지 한 칸씩 올리기
            a = parent[a];
            b = parent[b];
        }

        return a;
    }
}
```
