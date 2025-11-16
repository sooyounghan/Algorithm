-----
### 문제 60 - 최소 비용 구하기 (문제 1916번)
-----
1. 문제
```
N개의 도시가 있다. 그리고 한 도시에서 출발해 다른 도시에 도착하는 M개의 버스가 있다. 이들 버스의 요금은 각각 다르다.

A번쨰 도시에서 B번째 도시까지 가는 데 드는 최소 비용을 출력하라. 도시의 번호는 1부터 N까지다.
```

2. 입력
   - 1번째 줄에 도시의 개수 N(1 ≤ N ≤ 1,000)
   - 2번쨰 줄에 버스의 개수 M(1 ≤ M ≤ 100,000)이 주어짐
   - 그리고 3번째 줄에서 M + 2번쨰 줄까지 다음과 같은 버스의 정보가 주어짐
     + 가장 처음에는 버스의 출발 도시의 번호가 주어짐
     + 그 다음에는 도착지의 도시 번호가 주어지고, 이어서 버스 비용이 주어짐
     + 버스 비용은 0보다 크거나 같고, 100,000보다 작은 정수
   - 그리고 M + 3번째 줄에는 우리가 구하고자 하는 출발점의 도시 번호와 도착점의 도시 번호가 주어짐 : 출발점에서 도착점을 갈 수 있을 때만 입력으로 주어짐

3. 출력 : 1번째 줄에 출발 도시에서 도착 도시까지 가는 데 드는 최소 비용을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/1c71fe50-b3b5-4a7f-ba6c-f0e4723dca4e">
</div>

4. 1단계 : 문제 분석하기
   - 시작점과 도착점이 주어지고, 이 목적지까지 가는 최소 비용(최단 거리)을 구하는 문제
   - 또한, 버스 비용의 범위가 음수가 아니므로 이 문제는 다익스트라 알고리즘을 이용해 해결 가능
   - 도시의 개수가 최대 1,000개이므로 인접 행렬 방식으로도 그래프를 표현할 수 있지만, 시간 복잡도나 공간 효율성 측면을 고려해 인접 리스트 자료구조 선택

5. 2단계
   - 주어진 예제 데이터를 기반으로 그래프를 그림 (도시는 노드, 도시 간 버스 비용은 엣지로 나타냄)
<div align="center">
<img src="https://github.com/user-attachments/assets/750d8972-e1de-4119-b51c-e0ec3e3b0c99">
</div>

   - 첫째 숫자(도시 개수)의 크기만큼 인접 리스트 배열 크기를 설정
     + 이 때, 버스의 비용(가중치)이 존재하므로 인접 리스트 배열의 자료형이 될 클래스를 선언
     + 그리고 둘째 숫자(버스 개수)의 크기만큼 반복문을 돌면서 그래프를 리스트 배열에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/a875e41d-0867-4270-b15f-9959194f2b16">
</div>

   - 다익스트라 알고리즘을 수행 : 최단 거리 배열이 완성되면 정답 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/84f3a432-e896-48b4-b1de-4f9b8ccd75b1">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/8b68593a-cba9-4647-81f0-99b3d6397909">
</div>

7. 4단계 : 코드
```java
package graph.dijkstra;

import java.io.*;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class MinimumCost {
    public static int N, M;
    public static ArrayList<Node>[] list; // 인접 리스트로 그래프 표현
    public static int[] dist; // 최단 거리 배열
    public static boolean[] visit; // 사용 노드인지 확인하는 배열

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st;

        N = Integer.parseInt(br.readLine());
        M = Integer.parseInt(br.readLine());

        list = new ArrayList[N + 1];
        dist = new int[N + 1];
        visit = new boolean[N + 1];

        Arrays.fill(dist, Integer.MAX_VALUE); // 거리 배열을 충분히 큰 수로 초기화

        for(int i = 0; i <= N; i++) {
            list[i] =  new ArrayList<Node>();
        }

        for(int i = 0; i < M; i++) { // 주어진 그래프의 엣지를 인접 리스트 자료구조에 넣기
             st = new StringTokenizer(br.readLine());

             int start = Integer.parseInt(st.nextToken());
             int end = Integer.parseInt(st.nextToken());
             int weight = Integer.parseInt(st.nextToken());

             list[start].add(new Node(end, weight));
        }

        st = new StringTokenizer(br.readLine());

        int startIndex = Integer.parseInt(st.nextToken());
        int endIndex = Integer.parseInt(st.nextToken());
        bw.write(dikstra(startIndex, endIndex) + "\n"); // 다익스트라 알고리즘 수행
        bw.flush();
        bw.close();
        br.close();
    }

    public static int dikstra(int start, int end) {
        PriorityQueue<Node> pq = new PriorityQueue<Node>();

        pq.offer(new Node(start, 0));
        dist[start] = 0;

        while(!pq.isEmpty()) {
            Node nowNode = pq.poll();

            int now = nowNode.targetNode;

            if(!visit[now]) {
                visit[now] = true;

                for (Node n : list[now]) { // 선택 노드 + 비용 < 타깃 노드일 때, 값 업데이트
                    if(!visit[n.targetNode] && dist[n.targetNode] > dist[now] + n.value) {
                        dist[n.targetNode] = dist[now] + n.value;
                        pq.add(new Node(n.targetNode, dist[n.targetNode]));
                    }
                }
            }
        }
        return dist[end];
    }
}

class Node implements Comparable<Node> {
    int targetNode;
    int value;

    public Node(int targetNode, int value) {
        this.targetNode = targetNode;
        this.value = value;
    }

    @Override
    public int compareTo(Node o) {
        return this.value - o.value;
    }
}
```

8. 현재 사용할 수 있는 노드를 우선순위 큐 자료구조에 넣은 이유
   - 현재 연결된 노드 중 가장 적은 비용을 지니고 있는 노드를 빠르고, 간편하게 찾을 수 있음
   - 우선순위 큐는 데이터가 새롭게 드렁올 때마다 자동으로 정렬 : 정렬 기준은 Node 클래스에서 적절한 compareTo() 함수를 이용해 설정 가능
   - compareTo() 함수는 클래스의 정렬이 필요할 때 가장 보편적으로 사용하는 방식
