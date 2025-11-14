-----
### 문제 49 - 특정 거리의 도시 찾기 (문제 18352번)
-----
1. 문제
```
1번부터 N번까지의 도시와 M개의 단방향 도로가 존재하고, 모든 도로의 거리는 1인 도시가 있다.
도시 X로부터 출발해 도달할 수 있는 모든 도시 중 최단 거리가 정확히 K인 모든 도시들의 번호를 출력하시오,
(출발 도시 X에서 출발 도시 X로 가는 최단 거리는 항상 0이다.)

예를 들어 N = 4, K = 1, X = 1일 때, 다음과 같이 그래프가 구성되었다고 가정해보자.

이 때, 1번 도시에서 출발해 도달할 수 있는 도시 중 최단 거리가 1인 도시는 2번과 3번 도시다.
4번 도시는 최단 거리가 2이므로 출력하지 않는다.
```
<div align="center">
<img src="https://github.com/user-attachments/assets/0fd03259-ba4d-4a4e-b795-ef9ecfba2bae">
</div>

2. 입력
   - 1번째 줄에 도시의 개수(N), 도로의 개수(M), 거리 정보(K), 출발 도시의 번호(X)가 입력 (2 ≤ N ≤ 300,000, 1 ≤ M ≤ 1,000,000, 1 ≤ K ≤ 300,000, 1 ≤ X ≤ N)
   - 이후 M개의 줄에 걸쳐 2개의 자연수 A, B가 공백으로 구분되어 주어짐
   - A번 도시에서 B번 도시로 이동하는 단방향 도로가 존재한다는 뜻 (1 ≤ A, B ≤ N) (단, A와 B는 같을 수 없음)

3. 출력 : X로부터 출발해 도달 가능한 도시 중 최단 거리가 K인 모든 도시의 번호를 1줄에 1개씩 오름차순으로 출력하며, 해당하는 도시가 1개도 존재하지 않으면 -1을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/d8b6b550-3526-430c-8c4e-2f39455a6564">
</div>

4. 1단계 : 문제 분석
   - 모든 도로의 거리가 1이므로 가중치 없는 인접 리스트로 이 그래프 표현 가능
   - 도시의 개수가 300,000, 도로의 최대 크기가 1,000,000이므로 BFS 탐색을 수행하면 이 문제를 시간 복잡도 안에서 해결 가능

5. 2단계
   - 인접 리스트로 도시와 도로 데이터의 그래프를 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/4d0a5736-3455-4612-b588-5c3b1d938d39">
</div>

   - BFS 탐색 알고리즘으로 탐색을 수행하면서 각 도시로 가는 최단 거릿값을 방문 배열에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/63ef814a-5647-45bf-aa2a-6b4d1b60d999">
</div>

   - 최초에는 방문 도시가 1이고, 이동하지 않았으므로 방문 배열에 0을 저장
   - 이후 방문하는 도시는 이전 도시의 방문 배열값 + 1을 방문 배열에 저장하는 방식으로 이동 거리 저장
     + 예를 들어 방문 배열[2]에는 이전 도시의 방문 배열값이 0이므로 0 + 1을 기록

   - 탐색 종료 후 배열의 값이 K와 같은 도시의 번호 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/a53d1237-6f38-4e61-a722-9f1e0c11b3a3">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/6095a136-e47a-423b-ad08-35d818a4b3bc">
</div>

4. 코드
```java
package graph;

import java.util.*;

public class DistanceCertainCity {
    public static int visited[];
    public static ArrayList<Integer>[] A;
    public static int N, M, K ,X;
    public static List<Integer> answer;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        N = sc.nextInt(); // 노드의 수
        M = sc.nextInt(); // 엣지의 수
        K = sc.nextInt(); // 목표 거리
        X = sc.nextInt(); // 시작점
        
        A = new ArrayList[N + 1];
        answer = new ArrayList<>();
        
        for (int i = 1; i <= N; i++) {
            A[i] = new ArrayList<Integer>();
        }
        
        for (int i = 0; i < M; i++) {
            int S = sc.nextInt();
            int E = sc.nextInt();
            A[S].add(E);
        }
        
        visited = new int[N + 1]; // 방문 배열 초기화
        for (int i = 0; i <= N; i++) {
            visited[i] = -1;
        }
        
        BFS(X);
        
        for (int i = 0; i <= N; i++) {
            if(visited[i] == K) {
                answer.add(i);
            }
        }
        
        if(answer.isEmpty()) {
            System.out.println("-1");
        } else {
            Collections.sort(answer);
            for (int temp : answer) {
                System.out.println(temp);
            }
        }
    }

    // BFS 구현
    private static void BFS(int Node) {
        Queue<Integer> queue = new LinkedList<Integer>();
        
        queue.add(Node);
        visited[Node]++;
        
        while (!queue.isEmpty()) {
            int newNode = queue.poll();

            for (int i : A[newNode]) {
                if (visited[i] == -1) {
                    visited[i] = visited[newNode] + 1;
                    queue.add(i);
                }
            }
        }
    }
}
```
