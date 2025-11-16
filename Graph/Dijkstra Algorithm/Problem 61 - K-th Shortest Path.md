-----
### 문제 61 - K번쨰 최단 경로 찾기 (문제 1854번)
-----
1. 문제
```
봄 캠프를 마친 김 조교는 여러 도시를 돌며 여행을 다닐 계획이다.
그런데 김 조교는 '느림의 미학'을 중요시하는 사람이라 항상 최단 경로로만 이동하는 것을 별로 좋아하지 않는다.
하지만, 너무 시간이 오래 걸리는 경로도 그리 매력적인 것만은 아니어서 적당한 타협안인 'K번째 최단 경로'를 구하길 원한다.
그를 돕기 위한 프로그램을 작성해보자.
```

2. 입력
   - 1번째 줄에 n, m, k가 주어짐 (1 ≤ n ≤ 1,000, 0 ≤ m ≤ 200,000, 1 ≤ k ≤ 100) : n과 m은 각각 김 조교가 여행을 고려하고 있는 도시들의 개수와 도시 간에 존재하는 도로의 개수
   - 이어지는 m개의 줄에는 각각 도로의 정보를 제공하는 3개의 정수 a, b, c가 들어 있음 : 이는 a번 도시에서 b번 도시로 갈 때, c 시간이 걸린다는 의미 (1 ≤ a, b ≤ n, 1 ≤ c ≤ 1,000)
   - 도시의 번호는 1번부터 n번까지 연속한 숫자이고, 시작 도시는 1번

3. 출력
   - n개의 줄을 출력
   - i번쨰 줄에 1번 도시에서 i번 도시로 가는 K번째 최단 경로의 소요 시간을 출력 : 경로의 소요 시간은 경로 위에 있는 도로들을 따라 이동하는 데 필요한 시간들의 합
   - i번 도시에서 같은 i번 도시로 가는 최단 경로는 0이지만, 일반적으로 K번쨰 최단 경로는 0이 아닐 수 있음
   - 또, K번째 최단 경로가 존재하지 않으면 -1을 출력
   - 최단 경로에 같은 노드가 여러 번 포함될 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/adc54b8f-1824-4b30-ac2a-f2a38b43f2d6">
</div>

4. 1단계 : 문제 분석하기
   - 시작점과 도착점이 주어지고 목적지까지 가는 K번째 최단 경로를 구하는 문제
   - 도시(노드)의 개수는 1,000개, 도로(엣지)의 개수는 2,000,000개이면서 시간 제약이 2초이므로 다익스트라 알고리즘으로 접근
   - 최단 경로가 아닌 K번쨰 최단 경로를 구하는 것이 핵심
     + 최단 경로를 표현하는 배열을 우선순위 큐 배열(크기는 K)로 변경 : 최단 경로 ~ K번째 최단 경로까지 표현
     + 사용한 노드는 방문 배열에 확인해두고, 재사용하지 않는 부분은 삭제가 필요 : K번째 경로를 찾기 위해 노드를 여러 번 쓰는 경우가 발생할 수 있음

5. 2단계
   - 주어진 예제 데이터를 기반으로 그래프를 작성 (도시는 노드로, 도로는 에지로 나타냄)
<div align="center">
<img src="https://github.com/user-attachments/assets/98c10fd9-565f-4028-986f-320960bc3877">
</div>

   - 변수를 선언하고 그래프 데이터를 받는 부분은 모두 다익스트라 알고리즘 준비 과정과 동일
<div align="center">
<img src="https://github.com/user-attachments/assets/23a2208f-b5f2-424d-94e9-d26061e443a2">
</div>

   - 최단 거리 배열을 우선순위 큐 배열로 선언하고, 다음과 같은 기준을 세워 채워야 함
     + 예제에서 주어진 입력에서 K = 2이지만, 여기서는 K =3일 때 계산
     + 현재 노드에 저장된 경로가 K개 미마닝ㄹ 때 신규 경로를 추가
     + 경로가 K개일 때, 현재 경로 중 최대 경로와 신규 경로를 비교해 신규 경로가 더 작을 때 업데이트 : 우선순위 큐를 사용하면 이 로직을 쉽게 구현 가능
     + K번째 경로를 찾기 위해서는, 노드를 여러 번 쓰는 경우가 생기므로 사용한 노드는 방문 배열에 확인해두고 재사용하지 않는 부분은 삭제
<div align="center">
<img src="https://github.com/user-attachments/assets/905374da-36ab-4afd-a988-7d614e781346">
</div>

   - 최단 거리 배열을 탐색하면서 K번째 경로가 존재하면 출력하고, 존재하지 않으면 -1을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/76094d1b-9d1e-4e34-9fd7-d826a1db2fa0">
</div>

   - 우선순위 큐로 선언하면 편리한 점 : 이 부분에서 최단 거리 배열의 객체 형식을 우선순위 큐로 선언했으므로 새로운 노드가 삽입됐을 때 별도로 정렬하지 않아도 자동으로 정렬되어 편리하게 구현할 수 있음

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/4e261642-d853-4cbb-82ab-dd573acf3be2">
</div>

7. 코드
```java
package graph.dijkstra.kthshortestpath;

import java.io.*;
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class KthShortestPath {
    public static int INF = 100000000;

    public static void main(String[] args) throws IOException {
        int N, M, K;
        int[][] W = new int[1001][1001];

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        K = Integer.parseInt(st.nextToken());

        PriorityQueue<Integer>[] distQueue = new PriorityQueue[N + 1];
        Comparator<Integer> cp = new Comparator<Integer>() { // 오름차순 정렬 기준 생성
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 < o2 ? 1 : -1;
            }
        };

        for(int i = 0; i < N + 1; i++) {
            distQueue[i] = new PriorityQueue<>(K, cp);
        }

        for (int i = 0; i < M; i++) { // 인접 행렬에 그래프 데이터 저장
            st = new StringTokenizer(br.readLine());

            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());

            W[a][b] = c;
        }

        PriorityQueue<Node> pq = new PriorityQueue<>();

        pq.add(new Node(1, 0));
        distQueue[1].add(0);

        while (!pq.isEmpty()) {
            Node u = pq.poll();

            for(int adjNode = 1; adjNode <= N; adjNode++) {
                // 연결된 모든 노드로 검색하기 (시간 복잡도 측면에서 인접 행렬이 불리한 이유)
                if(W[u.node][adjNode] != 0) {
                    // 저장된 경로가 K가 안될 경우에는 추가
                    if(distQueue[adjNode].size() < K) {
                        distQueue[adjNode].add(u.cost + W[u.node][adjNode]);
                        pq.add(new Node(adjNode, u.cost + W[u.node][adjNode]));
                    }

                    // 저장된 경로가 K개이면, 현재 가장 큰 값보다 작을 때만 추가
                    else if(distQueue[adjNode].peek() > u.cost + W[u.node][adjNode]) {
                        distQueue[adjNode].poll(); // 기존 큐에서 Max 값 먼저 삭제해야 함
                        distQueue[adjNode].add(u.cost + W[u.node][adjNode]);
                        pq.add(new Node(adjNode, u.cost + W[u.node][adjNode]));
                    }
                }
            }
        }

        for(int i = 1; i <= N; i++) { // K번째 경로 출력
            if(distQueue[i].size()== K) {
                bw.write(distQueue[i].peek() + "\n");
            } else {
                bw.write(-1 + "\n");
            }
        }
        bw.flush();
        bw.close();
        br.close();
    }
}

// 노드 클래스 작성하기
class Node implements Comparable<Node> {
    int node;
    int cost;

    public Node(int node, int cost) {
        this.node = node;
        this.cost = cost;
    }

    @Override
    public int compareTo(Node o) {
        return this.cost < o.cost ? -1 : 1;
    }
}
```
