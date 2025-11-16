-----
### 문제 59 - 최단 경로 구하기 (문제 1753번)
-----
1. 문제
```
에지의 가중치가 10 이하의 자연수인 방향 그래프가 있다.
이 그래프의 시작점에서 다른 모든 노드로의 최단 경로를 구하시오.
```

2. 입력
   - 1번째 줄에 노드의 개수 V와 엣지의 개수 E가 주어짐 (1 ≤ V ≤ 20,000, 1 ≤ E ≤ 300,000) :  모든 노드는 1에서부터 V가지 번호가 매겨져 있음
   - 2번째 줄에 출발 노드의 번호 K가 주어짐 (1 ≤ K ≤ V)
   - 3번쨰 줄에서 E개의 줄에 걸쳐 각 에지의 정보(u, v, w)가 순서대로 주어짐 : 이는 u에서 v로 가는 가중치 w인 에지가 존재한다는 뜻으로, u와 v는 다름
   - 다 노드 사이에서 에지가 2개 이상 존재할 수 있음

3. 출력 : 1번째 줄부터 V개의 줄에 걸쳐, i번째 줄에 i번 노드까지 최단 경로값을 출력 (시작점은 0, 경로가 없을 때는 INF 출력)
<div align="center">
<img src="https://github.com/user-attachments/assets/879bd5b3-13a6-4dc0-8f87-418d8f8ee156">
</div>

4. 1단계 : 문제 분석하기
   - 시작점에서 다른 노드까지 최단 거리를 구하는 문제

5. 2단계
   - 인접 리스트에 노드를 저장하고 거리 배열을 초기화 : 거리 배열은 출발 노드는 0, 나머지는 무한으로 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/5fb9cfe3-1ee3-4ea0-8e9c-62b2024b6d6f">
</div>

   - 최초 시작점을 큐에 삽입하고, 다익스트라 알고리즘 수행
     + 거리 배열에서 아직 방문하지 않은 노드 중 현재 값이 가장 작은 노드를 서낵
     + 해당 노드와 연결된 노드들의 최단 거릿값을 다음 공식으로 업데이트
       * [연결 노드 거리 리스트 값]보다 [선택 노드의 거리 리스트 값 + 에지 가중치]가 더 작은 경우, 업데이트 수행
       * 업데이트가 수행되는 경우, 연결 노드를 우선 순위 큐에 삽입
     + 큐가 빌 때까지 위 과정 반복
<div align="center">
<img src="https://github.com/user-attachments/assets/4bd300d0-1aa3-45eb-85f7-59523664695f">
</div>

   - 완성된 거리 배열의 값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/c7d992a2-2e5b-4227-8065-2021cbd34649">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/34fb7a80-a45a-4b60-9984-792cca854ebd">
</div>

7. 코드
```java
package graph.dijkstra;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class ShortestPath {
    public static int V, E, K;
    public static int distance[];
    public static boolean visited[];
    public static ArrayList<Edge> list[];
    public static PriorityQueue<Edge> q = new PriorityQueue<>();

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(st.nextToken());
        K = Integer.parseInt(br.readLine());

        distance = new int[V + 1];
        visited = new boolean[V + 1];
        list = new ArrayList[V + 1];

        for(int i = 1; i <= V; i++ ) {
            list[i] = new ArrayList<Edge>();
        }

        for(int i = 1; i <= V; i++) {
            distance[i] = Integer.MAX_VALUE;
        }

        for(int i = 0; i < E; i++) { // 가중치가 있는 인접 리스트 초기화하기
            st = new StringTokenizer(br.readLine());

            int u = Integer.parseInt(st.nextToken());
            int v = Integer.parseInt(st.nextToken());
            int w = Integer.parseInt(st.nextToken());

            list[u].add(new Edge(v, w));
        }

        q.add(new Edge(K, 0)); // K를 시작점으로 설정하기
        distance[K] = 0;

        while(!q.isEmpty()) {
            Edge current = q.poll();

            int c_v = current.vertex;

            if(visited[c_v]) { // 이미 방문한적이 있는 노드는 다시 큐에 넣지 않음
                continue;
            }

            visited[c_v] = true;

            for(int i = 0; i < list[c_v].size(); i++) {
                Edge tmp = list[c_v].get(i);

                int next = tmp.vertex;
                int value = tmp.value;

                if(distance[next] > distance[c_v] + value) { // 최소 거리로 업데이트
                    distance[next] = distance[c_v] + value;
                    q.add(new Edge(next, distance[next]));
                }
            }
        }

        for(int i = 1; i <= V; i++) { // 거리 배열 출력
            if(visited[i]) {
                System.out.println(distance[i]);
            } else {
                System.out.println("INF");
            }
        }
    }
}

class Edge implements Comparable<Edge> {
    int vertex, value;

    public Edge(int vertex, int value) {
        this.vertex = vertex;
        this.value = value;
    }

    @Override
    public int compareTo(Edge e) {
        if(this.value > e.value) return 1;
        else return -1;
    }
}
```
