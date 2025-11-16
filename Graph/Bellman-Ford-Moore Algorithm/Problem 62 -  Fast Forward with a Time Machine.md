-----
### 문제 62 - 타임머신으로 빨리 가기 (문제 11657번)
-----
1. 문제
```
N개의 도시와 한 도시에 출발해 다른 도시에 도착하는 버스가 M개 있다.

각 버스 정보는 A, B, C로 나타낼 수 있는데, A는 시작 도시, B는 도착 도시, C는 버스를 타고 이동하는 데 걸리는 시간이다.
시간 C가 양수가 아닐 때가 있다. C = 0일 경우에는 순간 이동을 할 때, C < 0일 경우에는 타임머신으로 시간을 되돌아갈 때다.

1번 도시에서 출발해 나머지 도시로 가는 가장 빠른 시간을 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 도시의 개수 N(1 ≤ N ≤ 500), 버스 노선의 개수 M (1 ≤ M ≤ 6,000)이 주어짐
   - 2번쨰 줄부터는 M개의 줄에는 버스 노선의 정보 A, B, C(1 ≤ A, B ≤ N, -10,000 ≤ C ≤ 10,000)가 주어짐

3. 출력
   - 만약 1번 도시에서 출발해 어떤 도시로 가는 과정에서 시간을 무한히 오래 전으로 되돌릴 수 있다면, 1번째 줄에 -1을 출력
   - 그렇지 않다면 N - 1개 줄에 걸쳐 각 줄에 1번 도시에서 출발해 2번 도시, 3번 도시, ..., N번 도시로 가는 가장 빠른 시간을 순서대로 출력
   - 만약 이 도시로 가는 경로가 없다면 -1을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/debcd3c3-10cd-4453-826d-265f17158957">
</div>

4. 1단계 : 문제 분석하기
   - 시작점 및 다른 노드와 관련된 최단 거리를 구하는 문제이지만, 특이한 점은 엣지에 해당하는 이동하는 시간이 양수가 아닌 0 또는 음수가 가능하다는 것
   - 이렇게 시작점에서 다른 노드와 관련된 최단 거리를 구하는데, 엣지가 음수가 가능할 때는 벨만-포드 알고리즘을 사용

5. 2단계
   - 엣지 리스트에서 엣지 데이터를 저장한 후 거리 배열을 다음과 같이 초기화 : 최초 시작점에 해당하는 거리 배열값은 0으로 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/a3144e03-ae4c-4632-9e92-de85744e960a">
</div>

   - 다음 순서에 따라 벨만-포드 알고리즘 수행
     + 모든 엣지에 관련된 정보를 가져온 후 다음 조건에 따라 거리 배열의 값을 업데이트
       * 출발 노드가 방문한 적이 없는 노드(출발 노드 거리 == INF)일 때, 값을 업데이트 하지 않음
       * 출발 노드의 거리 배열 값 + 엣지 가중치 < 종료 노드의 거릿 배열값일 때, 종료 노드의 거리 배열값을 업데이트
     + '노드 개수 - 1'번 만큼 첫 번째 단계를 반복
     + 음수 사이클 유무를 알기 위해 모든 엣지에 관해 다시 한 번 첫 번째 단계를 수행 : 이 때, 한 번이라도 값이 업데이트 되면 음수 사이클이 존재한다고 판단

   - 실제로 수행할 때는 엣지가 저장된 순서에 따라 동작하므로 거리 배열의 값이 다음과 같이 업데이트 (이론의 업데이트와 다르지만, 알고리즘에 큰 영향을 미치지 않고, 실제 코드 디버그값과 이론에서의 값이 달라 혼동할 수 있으므로, 배열을 실제 코드 기준으로 업데이트)
<div align="center">
<img src="https://github.com/user-attachments/assets/bdb70dd0-69ec-4da0-bb26-75e3916c57d0">
</div>

   - 음수 사이클이 존재하면 -1, 존재하지 않으면 거리 배열의 값을 출력 : 단, 거리 배열의 값이 INF일 경우 -1을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/201b48dc-182c-4ac7-b1a6-7d8150aa9e26">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/62f1d6df-b979-4285-8009-d15117b20b56">
</div>

7. 코드
```java
package graph.bellmanford;

import java.io.*;
import java.util.Arrays;
import java.util.StringTokenizer;

public class TimeMachine {
    private static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    private static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    public static int N, M;
    public static long distance[];
    public static Edge edges[];

    public static void main(String[] args) throws IOException {
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        edges = new Edge[M + 1];
        distance = new long[N + 1];

        Arrays.fill(distance, Integer.MAX_VALUE); // 최단 거리 배열 초기화

        for(int i = 0; i < M; i++) { // 엣지 리스트에 데이터 저장
            st = new StringTokenizer(br.readLine());

            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            int time = Integer.parseInt(st.nextToken());

            edges[i] = new Edge(start, end, time);
        }

        // 벨만-포드 알고리즘 수행
        distance[1] = 0;

        for(int i = 1; i < N; i++) { // N보다 1개 적은 수만큼 반복
            for(int j = 0; j < M; j++) {
                Edge edge = edges[j];

                // 더 작은 최단 거리가 있을 때 업데이트
                if(distance[edge.start] != Integer.MAX_VALUE && distance[edge.end] > distance[edge.start] + edge.time) {
                    distance[edge.end] = distance[edge.start] + edge.time;
                }
            }
        }

        boolean mCycle = false;

        for(int i = 0; i < M; i++) { // 음수 사이클 확인
            Edge edge = edges[i];

            if(distance[edge.start] != Integer.MAX_VALUE && distance[edge.end] > distance[edge.start] + edge.time) {
                mCycle = true;
            }
        }

        if(!mCycle) { // 음수 사이클이 없을 때
            for(int i = 2; i <= N; i++) {
                if(distance[i] == Integer.MAX_VALUE) {
                    System.out.println("-1");
                } else {
                    System.out.println(distance[i]);
                }
            }
        } else { // 음수 사이클이 있을 때
            System.out.println("-1");
        }
    }
}

// 엣지 리스트를 편하게 다루기 위한 클래스
class Edge {
    int start, end, time; // 시작점, 도착점, 걸리는 시간

    public Edge(int start, int end, int time) {
        this.start = start;
        this.end = end;
        this.time = time;
    }
}
```
