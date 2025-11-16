-----
### 문제 64 - 가장 빠른 버스 노선 구하기 (문제 11404번)
-----
1. 문제
```
n(2 ≤ n ≤ 100)개의 도시가 있다.
그리고 한 도시에서 출발해 다른 도시에 도착하는 m(1 ≤ m ≤ 100,000)개의 버스가 있다.
각 버스는 한 번 사용할 때 필요한 비용이 있다.

모든 도시의 쌍 (A, B)에 관해 도시 A에서 B로 가는 데 필요한 버스의 최솟값을 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 도시의 개수 n이 주어짐
   - 2번째 줄에 버스의 개수 m이 주어짐
   - 3번쨰 줄에서 m + 2줄까지 다음과 같은 버스의 정보가 주어짐
     + 버스의 정보는 버스의 시작 도시 a, 도착 도시 b, 한 번 더 타는 데 필요한 비용 c로 구성
     + 시작 도시와 도착 도시가 같은 경우는 없음
     + 비용은 100,000보다 작거나 같은 자연수
     + 시작 도시와 도착 도시를 연결하는 노선은 1개가 아닐 수 있음

3. 출력 : n개의 줄을 출력해야 함 (i번째 줄에 출력하는 j번째 숫자는 도시 i에서 j로 가는 데 필요한 최소 비용이며, 만약, i에서 j로 갈 수 없을 때는 그 자리에 0을 출력)
<div align="center">
<img src="https://github.com/user-attachments/assets/1dc3310d-ef88-46c6-b9b1-93f3896d7e9b">
</div>

4. 1단계 : 문제 분석하기
   - 모든 도시 쌍과 관련된 최솟값을 찾아야 하는 문제
   - 그래프에서 시작점을 지정하지 않고, 모든 노드와 관련된 최소 경로를 구하는 알고리즘이 바로 플로이드-워셜 알고리즘
   - 도시의 최대 개수가 100개로 매우 작은 편이므로 O($N^{3}$)의 시간 복잡도의 플로이드-워셜 알고리즘으로 해결 가능

5. 2단계
   - 버스 비용 정보를 인접 행렬로 저장 : 먼저 인접 행렬을 초기화하고, 연결 도시가 같으면 (i == j) 0, 아니면 충분히 큰 수로 값을 초기화하며, 주어진 버스 비용 데이터 값을 인접 행렬에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/c38fe915-0c04-47cd-a1ae-270f8ab9538b">
</div>

   - 플로이드-워셜 알고리즘을 수행 : 다음 점화식을 활용한 3중 for문으로 모든 중간 경로 탐색
<div align="center">
<img src="https://github.com/user-attachments/assets/d1a4e68e-f1bc-4bf3-9b74-64b55df67221">
</div>

   - 알고리즘으로 변경된 인접 행렬을 출력 : 인접 행렬 자체가 모든 쌍의 최단 경로를 나타내는 정답 배열로, 정답 배열 그대로 출력할 때, 문제 요구사항에 따라 도착 도시에 도착하지 못할 때(∞)는 0, 아닐 때는 배열의 값 출력

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/dec8f90d-5ced-4a69-9ab1-c5d05c950879">
</div>

7. 코드
```java
package graph.floydwarshall;

import java.io.*;
import java.util.StringTokenizer;

public class FastestBusRoute {
    private static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    private static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    public static int N, M;
    public static int distance[][];

    public static void main(String[] args) throws IOException {
        N = Integer.parseInt(br.readLine());
        M = Integer.parseInt(br.readLine());
        distance = new int[N + 1][N + 1];

        for (int i = 1; i <= N; i++) { // 인접 행렬 초기화
            for (int j = 1; j <= N; j++) {
                if(i == j) {
                    distance[i][j] = 0;
                } else {
                    distance[i][j] = 100000001; // 충분히 큰 수로 저장
                }
            }
        }

        for (int i = 0; i < M; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());

            int s = Integer.parseInt(st.nextToken());
            int e = Integer.parseInt(st.nextToken());
            int v = Integer.parseInt(st.nextToken());

            if (distance[s][e] > v) {
                distance[s][e] = v;
            }
        }

        // 플로이드-워셜 알고리즘 수행
        for (int k = 1; k <= N; k++) {
            for (int i = 1; i <= N; i++) {
                for (int j = 1; j <= N; j++) {
                    if (distance[i][j] > distance[i][k] + distance[k][j]) {
                        distance[i][j] = distance[i][k] + distance[k][j];
                    }
                }
            }
        }

        for (int i = 1; i <= N; i++) {
            for (int j = 1; j <= N; j++) {
                if (distance[i][j] == 100000001) {
                    System.out.print("0 ");
                } else {
                    System.out.print(distance[i][j] + " ");
                }
            }
            System.out.println();
        }
    }
}
```
