-----
### 문제 58 - 임계 경로 구하기 (문제 1948번)
-----
1. 문제
```
월드 나라는 모든 도로가 일방통행이고, 사이클이 없다.
그런데 무수히 많은 사람이 월드 나라의 지도를 그리기 위해 어떤 시작 도시에서 도착 도시까지 갈 수 있는 모든 경로를 탐색하고자 한다.

이 지도를 그리는 사람들은 사이가 너무 좋아서 지도를 그리는 일을 모두 마치고 도착 도시에서 만나기로 했다.
어떤 사람은 도착 시간에 늦지 않기 위해 1분도 쉬지 않고 달려야 한다.

이들이 출발 도시에서 출발한 후 도착 도시에서 만나기까지 걸리는 최소 시간과, 1분도 쉬지 않고 달려야 하는 사람들이 지나는 도로의 수를 계산하는 프로그램을 작성하시오.
(출발 도시는 돌아오는 도로가 0개, 도착 도시는 나가는 도로가 0개이다.)
```

2. 입력
   - 1번쨰 줄에 도시의 개수 n (1 ≤ n ≤ 10,000)
   - 2번째 줄에 도로의 개수 m (1 ≤ m ≤ 100,00)이 주어짐
   - 3번째 줄에 m + 2줄까지 다음과 같은 도로 정보가 주어짐
     + 처음에는 도로의 출발 도시의 번호가 주어지고, 그 다음에는 도착 도시의 번호 그리고 마지막에는 이 도로를 지나는 데 걸리는 시간이 주어짐
     + 도로를 지나가는 시간은 10,000보다 작거나 같은 자연수
     + 그리고 m + 3째 줄에는 지도를 그리는 사람들이 출발하는 출발 도시와 도착 도시가 주어짐
   - 모든 도시는 출발 도시에서 도달할 수 있고, 모든 도시에서 도착 도시에 도달할 수 있음

3. 출력 : 1번째 줄에 이들이 만나는 시간, 2번째 줄에 1분도 쉬지 않고 달려야 하는 도로의 개수가 몇 개 인지 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/147b9ae8-068d-4bb0-b7d3-4c09812b9c57">
</div>

4. 1단계 : 문제 분석하기
   - 출발 도시와 도착 도시가 주어지기 때문에 일반적인 위상 정렬이 아닌 시작점을 출발 도시로 지정하고 위상 정렬을 수행하면, 출발 도시에서 도착 도시까지 거치는 모든 도시와 관련된 임계 경로 값을 구할 수 있음
   - 단, 이 문제의 핵심은 1분도 쉬지 않고 달려야 하는 도로의 수를 구하는 것 : 엣지 뒤집기라는 아이디어 필요 (그래프 문제에서 종종 나오는 개념)

5. 2단계
   - 인접 리스트에 노드 데이터를 저장하고, 진입 차수 배열 값을 업데이트 : 이 때, 엣지의 방향이 반대인 역방향 인접 리스트도 함께 생성하고 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/bcd19ebc-df30-49d9-9c25-9e6bf9b5a189">
</div>

   - 시작 도시에서 위상 정렬을 수행해 각 도시와 관련된 임계 경로를 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/c1bc4de8-f7cd-480b-8c90-e9431e07fbb7">
</div>

   - 도착 도시에서 역방향으로 위상 정렬을 수행 : 이 때, '이 도시의 임계 경로값 + 도로 시간(에지) == 이전 도시의 임계 경로값'일 경우에는 이 도로를 1분도 쉬지 않고 달려야 하는 도로로 카운팅하고, 이 도시를 큐에 삽입하는 로직 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/c74649b6-2b03-4489-8fc8-6a9da5a4c0f6">
</div>

   - 도착 도시의 임계값 경로(12)과 1분도 쉬지 않고 달려야 하는 도로의 수(5)를 출력

   - 노드를 큐에 삽입할 때 주의할 점
     + 1분도 쉬지 않고 달려야하는 도로로 이어진 노드와 연결된 다른 도로만이 1분도 쉬지 않고 달려야 하는 도로의 후보가 될 수 있으므로 이 메커니즘을 바탕으로 노드를 큐에 삽입해야 함
     + 또한, 중복으로 도로를 카운트하지 않기 위해 이미 방문한 적이 있는 노드는 큐에 넣어 주지 않음

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/38f7eeb3-162c-4069-ba9b-e90731b28d11">
</div>

7. 코드
```java
package graph.topologysort;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class CriticalPath {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());
        int M = Integer.parseInt(br.readLine());

        ArrayList<ArrayList<dNode>> A = new ArrayList<>();
        ArrayList<ArrayList<dNode>> reverseA = new ArrayList<>();

        for (int i = 0; i <= N; i++) {
            A.add(new ArrayList<>());
            reverseA.add(new ArrayList<>());
        }

        int[] indegree = new int[N + 1]; // 진입 차수 배열
        for(int i = 0; i < M; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());

            int S = Integer.parseInt(st.nextToken());
            int E = Integer.parseInt(st.nextToken());
            int V = Integer.parseInt(st.nextToken());

            A.get(S).add(new dNode(E, V));
            reverseA.get(E).add(new dNode(S, V)); // 역방향 엣지 정보 저장
            indegree[E]++; // 진입 차수 배열 초기 데이터 저장
        }

        StringTokenizer st = new StringTokenizer(br.readLine());

        int startDosi = Integer.parseInt(st.nextToken());
        int endDosi = Integer.parseInt(st.nextToken());

        // 위상 정렬
        Queue<Integer> queue = new LinkedList<>();

        queue.offer(startDosi);

        int[] result = new int[N + 1];

        while(!queue.isEmpty()) {
            int now = queue.poll();

            for (dNode next : A.get(now)) {
                indegree[next.targetNode]--;

                result[next.targetNode] = Math.max(result[next.targetNode], result[now] + next.value);

                if(indegree[next.targetNode] == 0) {
                    queue.offer(next.targetNode);
                }
            }
        }

        // 위상 정렬 Reverse
        int resultCount = 0;
        boolean[] visited = new boolean[N + 1];

        queue = new LinkedList<>();
        queue.offer(endDosi);
        visited[endDosi] = true;

        while(!queue.isEmpty()) {
            int now = queue.poll();

            for (dNode next : reverseA.get(now)) {
                // 1분도 쉬지 않는 도로 확인
                if (result[next.targetNode] + next.value == result[now]) {
                    resultCount++;

                    // 중복 카운트 방지를 위해 이미 방문한 적 있는 노드 제외
                    if (visited[next.targetNode] == false) {
                        visited[next.targetNode] = true;
                        queue.offer(next.targetNode);
                    }
                }
            }
        }
        System.out.println(result[endDosi]);
        System.out.println(resultCount);
    }
}

class dNode {
    int targetNode;
    int value;

    public dNode(int targetNode, int value) {
        this.targetNode = targetNode;
        this.value = value;
    }
}
```
