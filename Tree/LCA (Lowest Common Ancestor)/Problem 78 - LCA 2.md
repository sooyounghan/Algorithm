-----
### 문제 78 - 최소 공통 조상 구하기 2 (문제 11438번)
-----
1. 문제
```
N(2 ≤ N ≤ 100,000)개의 노드로 이뤄진 트리가 주어진다.
트리의 각 노드는 1번부터 N번까지 번호가 매겨져 있으며, 루트는 1번이다.

두 노드의 쌍 M(1 ≤ M ≤ 100,000)개가 주어졌을 때, 두 노드의 가장 가까운 공통 조상이 몇 번인지 출력하시오.
```

2. 입력
   - 1번쨰 줄에 노드의 개수 N이 주어짐
   - 그 다음 N - 1개 줄에는 트리상에서 연결된 두 노드가 주어짐
   - 그 다음 줄에는 가장 가까운 공통 조상을 알고 싶은 쌍의 개수 M이 주어짐
   - 그 다음 M개의 줄에는 노드 쌍이 주어짐

3. 출력 : M개의 줄에 차례대로 입력받은 두 노드의 가장 가까운 공통 조상을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/5f410aff-bcd3-4bfb-9d55-0b860054d7f1">
</div>

4. 1단계 : 문제 분석하기
   - 기존 LCA 문제보다 노드의 개수(M)의 개수가 매우 커진 것을 알 수 있음
   - 그렇기 때문에, 일반적인 최소 공통 조상 구하기 방식으로 구현하면 시간 초과가 발생 : 따라서, 거듭제곱 형태를 이용한 최소 공통 조상 빠르게 구하기 방식으로 문제 해결
  
5. 2단계
   - 인접 리스트로 트리 데이터 구현
   - 탐색 알고리즘(DFS, BFS)를 이용해 각 노드의 깊이를 구함
<div align="center">
<img src="https://github.com/user-attachments/assets/fd5938c6-9cf8-4a5e-b589-3d25a5cdc3b7">
</div>

   - 점화식을 이용해 Parent 배열(부모 노드 배열)을 구함
<div align="center">
<img src="https://github.com/user-attachments/assets/a7831164-0f66-49b5-955b-9e65c17bd84b">
</div>

   - 깊이가 깊은 노드는 Parent 배열을 이용해 $2^{k}$만큼 빠르게 이동시켜 깊이를 맞춤
     + 깊이가 2만큼 차이($2^{1}$)가 나이므로 15번 노드를 15의 $2^{1}$번쨰 부모 노드인 5로 변경해 깊이를 맞춤
     + 한 칸이 오르는 것이 아닌 2의 거듭제곱으로 빠르게 올라감
    
   - 부모 노드를 올라가면서 최소 공통 조상을 찾음
     + Parent 배열을 이용해 $2^{k}$만큼 넘어가면서 찾는 것이 핵심
     + k는 dpeth의 최댓값에서 1씩 감소
<div align="center">
<img src="https://github.com/user-attachments/assets/be9d8c89-942a-4ab2-8668-aadb6272d3b5">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/a39f3f06-f5f7-400b-a39d-8a51a4f83645">
</div>

7. 코드
```java
package tree.lca;

import org.w3c.dom.Node;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class LCA2 {
    public static ArrayList<Integer>[] tree;
    public static int[] depth;
    public static int kmax;
    public static int[][] parent;
    public static boolean[] visited;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine()); // 노드의 수
        tree = new ArrayList[N + 1];

        for(int i = 1; i <= N; i++) {
            tree[i] = new ArrayList<Integer>();
        }

        for(int i = 0; i < N - 1; i++) { // 인접 리스트에 그래프 데이터 저장
            StringTokenizer st = new StringTokenizer(br.readLine());

            int s = Integer.parseInt(st.nextToken());
            int e = Integer.parseInt(st.nextToken());

            tree[s].add(e);
            tree[e].add(s);
        }

        depth = new int[N + 1];
        visited = new boolean[N + 1];

        int temp = 1;
        kmax = 0;

        while(temp <= N) { // 최대 가능 depth 구하기
            temp <<= 1;
            kmax++;
        }

        parent = new int[kmax + 1][N + 1];

        BFS(1);

        for(int k = 1; k <= kmax; k++) {
            for (int n = 1; n <= N; n++) {
                parent[k][n] = parent[k - 1][parent[k -1][n]];
            }
        }

        int M = Integer.parseInt(br.readLine()); // 질의의 수
        for (int i = 0; i < M; i++) {
            // 공통 조상을 구할 노드
            StringTokenizer st = new StringTokenizer(br.readLine());

            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            int LCA = executeLCA(a, b);
            System.out.println(LCA);
        }
    }

    // BFS 구현하기
    private static void BFS(int node) {
        Queue<Integer> queue = new LinkedList<Integer>();
        queue.add(node);
        visited[node] = true;

        int level = 1;
        int now_size = 1;
        int count = 0;

        while(!queue.isEmpty()) {
            int nowNode = queue.poll();

            for (int next : tree[nowNode]) {
                if(!visited[next]) {
                    visited[next] = true;
                    queue.add(next);

                    parent[0][next] = nowNode; // 부모 노드 저장
                    depth[next] = level; // 노드 depth 저장
                }
            }

            count++;

            if(count == now_size) {
                count = 0;
                now_size = queue.size();
                level++;
            }
        }
    }

    static int executeLCA(int a, int b) {
        if(depth[a] > depth[b]) { // 더 깊은 depth가 b가 되도록 변경
            int temp = a;
            a = b;
            b = temp;
        }

        for (int k = kmax; k >= 0; k--) { // depth를 빠르게 맞추기
            if(Math.pow(2, k) <= depth[b] - depth[a]) {
                if (depth[a] <= depth[parent[k][b]]) {
                    b = parent[k][b];
                }
            }
        }

        for (int k = kmax; k >= 0; k--) { // 조상 빠르게 찾기
            if (parent[k][a] != parent[k][b]) {
                a = parent[k][a];
                b = parent[k][b];
            }
        }

        int LCA = a;
        if (a != b) {
            LCA = parent[0][LCA];
        }

        return LCA;
    }
}
```
