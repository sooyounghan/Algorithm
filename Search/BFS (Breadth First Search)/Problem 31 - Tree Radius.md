-----
### 문제 31 - 트리의 지름 구하기 (문제 1167번)
-----
1. 문제
```
트리의 지름은 트리를 구성하는 노드 중 두 노드 사이의 거리가 가장 긴 것을 의미한다.
트리의 지름을 구하시오.
```

2. 입력
   - 1번째 줄에서는 트리의 노드 개수 V (2 ≤ V ≤ 100,000)
   - 2번째 줄부터 V개의 줄에 걸쳐 엣지의 정보가 주어짐
   - 먼저 노드 번호가 주어지고, 그다음으로 연결된 엣지의 정보를 의미하는 정수가 2개씩(연결된 노드 번호, 거리) 주어짐 (거리는 10,000이하 자연수)
   - 예를 들어 2번째 줄에 3 1 2 4 3 -1이 주어졌을 떄, 노드 3은 노드 1과 거리가 2인 에지로 연결되어 있고, 노드 4와 거리가 3인 엣지로 연결되어 있다는 뜻이며, -1은 더 이상 노드가 없으므로 종료한다는 의미

3. 출력 : 트리 지름 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/96bacc9f-9ac8-4b8d-b015-e747e0735200">
</div>

4. 1단계 : 문제 분석하기
   - 가장 긴 경로를 찾는 방법 : 임의의 노드에서 가장 긴 경로로 연결되어 있는 노드는 트리의 지름에 해당하는 두 노드 중 하나

5. 2단계
   - 그래프를 인접 리스트로 저장 : (노드, 가중치)로 표현하기 위해 노드는 클래스로 선언
<div align="center">
<img src="https://github.com/user-attachments/assets/ff680b0e-8278-4fd9-a49b-47903e05ea6f">
</div>

   - 임의의 노드에서 BFS를 수행하고 탐색할 때 각 노드의 거리를 배열에 저장 : 여기에서는 임의의 노드 중 노드 2에 탐색을 시작하는 경우
<div align="center">
<img src="https://github.com/user-attachments/assets/8f3bac3f-77b1-4f5a-bf07-9ef6d2a8e8a3">
</div>

   - 그림을 보면 2에서 출발하므로, distance[2] = 0
     + 2는 4와 연결되어 있으므로 4를 방문
     + 2와 4의 거리는 4이므로 distance[4]에 현재 노드의 거리 + 엣지 길이, 즉, distance[2] + 4를 기록
     + 그 결과 distance[4]에는 4가 기록

   - 이어서 4는 2, 3, 5와 연결되어 있음
     + 그런데 2는 방문했으므로 3과 5를 방문
     + 4에서 3으로 향하는 엣지 길이는 3이므로 distance[3]에 distance[4] + 3을 기록
     + 그 결과 distance[3]에는 7이 기록

   - 이런 방식으로 노드를 방문하여 거리 배열 업데이트

   - 위 과정에서 얻은 배열에서 임의의 노드와 가장 먼 노드를 찾음
     + 그런 다음 그 노드로부터 BFS를 다시 수행
     + 마찬가지로 탐색할 때 각 노드의 거리를 배열에 저장 : 현재의 경우 distance[5]가 가장 크므로 BFS를 다시 수행
<div align="center">
<img src="https://github.com/user-attachments/assets/519e4c20-c012-41e8-9550-2d16f98f19cf">
</div>

   - distance 업데이트 방식은 위 과정과 같음 : 업데이트 결과는 11, 10, 9, 6, 0
   - 위 과정에서 배열에 저장한 값 중 가장 큰 값을 이 트리의 지름으로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/03f4800e-814f-4de4-923e-f3d7647f3b91">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/93817e70-ca4f-4831-b195-21a2cd6e7342">
</div>

7. 코드
```java
package search.bfs;

import java.util.*;

public class TreeRadius {
    public static boolean visited[];
    public static int[] distance;
    public static ArrayList<Edge>[] A;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt(); // 노드 개수
        A = new ArrayList[N + 1];

        for (int i = 1; i <= N; i++) {
            A[i] = new ArrayList<Edge>();
        }

        for (int i = 0; i < N; i++) { // A 인접 리스트에 그래프 데이터 저장
            int S = sc.nextInt();

            while(true) {
                int E = sc.nextInt();
                if(E == -1) {
                    break;
                }

                int V = sc.nextInt();
                A[S].add(new Edge(E, V));
            }
        }

        distance = new int[N + 1];
        visited = new boolean[N + 1];

        BFS(1);

        int Max = 1;

        for(int i = 2; i <= N; i++) { // distance 배열에서 가장 큰 값으로 다시 시작점 설정
            if (distance[Max] < distance[i]) {
                Max = i;
            }
        }

        distance = new int[N + 1];
        visited = new boolean[N + 1];
        BFS(Max); // 새로운 시작점으로 실행하기
        Arrays.sort(distance);
        System.out.println(distance[N]);
    }

    private static void BFS(int index) { // BFS 구현하기
        Queue<Integer> queue = new LinkedList<Integer>();
        queue.add(index);
        visited[index] = true;

        while(!queue.isEmpty()) {
            int nowNode = queue.poll();

            for (Edge i : A[nowNode]) {
                int e = i.e;
                int v = i.value;

                if(!visited[e]) {
                    visited[e] = true;
                    queue.add(e);
                    distance[e] = distance[nowNode] + v; // 거리 배열 업데이트
                }
            }
        }
    }
}

class Edge {
    int e;
    int value;

    public Edge(int e, int value) {
        this.e = e;
        this.value = value;
    }
}
```
