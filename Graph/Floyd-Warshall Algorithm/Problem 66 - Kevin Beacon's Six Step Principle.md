-----
### 문제 66 - 케빈 베이컨의 6단계 법칙 (문제 1389번)
-----
1. 문제
```
케인 베이컨의 6단계 법칙에 따르면 지구에 있는 모든 사람은 최대 6단계 이내에서 서로 아는 사람으로 연결될 수 있다.
케빈 베이컨의 게임은 임의의 두 사람이 최소 몇 단계 만에 이어질 수 있는지 계산하는 게임이다.
케빈 베이컨의 수는 모든 사람과 케빈 베이컨 게임을 했을 때 나오는 단계의 합이다.

예를 들어, 백준 온라인 저지의 유저가 5명, 1과 3, 1과 4, 2와 3, 3과 4, 4와 5가 친구일 때를 생각해보자.
   - 1은 2까지 3을 이용해 2단계, 3까지 1단계, 4까지 1단계, 5까지 4를 이용해 2단계 만에 알 수 있다. 따라서 케빈 베이컨의 수는 2 + 1 + 1 + 2 = 6
   - 2는 1까지 3을 이용해 2단계, 3까지 1단계, 4까지 3을 이용해 2단계, 5까지 3과 4를 이용해 3단계 만에 알 수 있다. 따라서 케빈 베이컨의 수는 2 + 1 + 2 + 3 = 8이다.
   - 3은 1까지 1단계, 2까지 1단계, 4까지 1단계, 5까지 4를 이용해 2단계만에 알 수 있다. 따라서 케빈 베이컨의 수는 1 + 1 + 1 + 2 = 5이다.
   - 4는 1까지 1단계, 2까지 3을 이용해 2단계, 3까지 1단계, 5까지 1단계만에 알 수 있다. 4의 케빈 베이컨 수는 1 + 2 + 1 + 1 = 5가 된다.
   - 5는 1까지 4를 이용해 2단계, 2까지 4와 3을 이용해 3단계, 3까지 4를 이용해 2단계, 4까지 1단계만에 알 수 있다. 5의 케빈 베이컨의 수는 2 + 3 + 2 + 1 = 8이다.

즉, 5명의 유저 중 케빈 베이컨의 수가 가장 작은 사람은 3, 4이다.
위와 같이 백준 온라인 저지의 유저 수와 친구 관계가 입력으로 주어졌을 때 케빈 베이컨의 수가 가장 작은 사람을 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 유저의 수 N(2 ≤ N ≤ 100)과 친구 관계의 수 M(1 ≤ M ≤ 5,000)이 주어짐
   - 2번째 줄부터 M개의 줄에는 친구 관계가 주어짐
     + 친구 관계는 A와 B로 이뤄져 있으며, A와 B가 친구라는 뜻임
     + A와 B가 친구이면 B와 A도 친구이며, A와 B가 같은 경우는 없음
     + 친구 관게는 중복되어 들어올 수 있으며, 친구가 1명도 없는 사람은 없음
     + 또한, 모든 사람은 친구 관계로 연결되어 있음
   - 사람의 번호는 1부터 N까지이고, 두 사람이 같은 번호일 경우는 없음

3. 출력 : 1번쨰 줄에 백준 온라인 저지의 유저 중 케빈 베이컨의 수가 가장 적은 사람을 출력 (그런 사람이 여러 명일 경우, 번호가 가장 작은 사람을 출력)
<div align="center">
<img src="https://github.com/user-attachments/assets/680305cf-2afb-4436-8e54-f0593214830d">
</div>

4. 1단계 : 문제 분석하기
   - BFS 탐색 알고리즘을 이용해도 해결할 수 있는 문제
   - 하지만 유저의 최대 수가 100 정도로 작기 때문에 플로이드-워셜 알고리즘으로 해결 가능
   - 이를 위한 몇 가지 아이디어
     + 직접적인 친구 관계를 맺은 상태를 비용 1로 게산하는 것 : 즉, 가중치를 1로 정한 후 인접 행렬에 저장한다는 의미
     + 또한, 플로이드-워셜은 모든 쌍과 관련된 최단 경로이므로 한 row의 배열값은 해당 row의 index 값이 가리키는 다른 모든 노드와 관련된 최단 경로를 나타낸다고 볼 수 있음
     + 즉, i번째 row의 합이 i번째 사람의 케빈 베이컨의 수

5. 2단계
   - 먼저 인접 행렬을 생성한 후, 자기 자신이면(i == j) 0, 아니면 충분히 큰 수로 인접 행렬의 값을 초기화하고, 주어진 친구 관게를 인접 행렬에 저장 : i와 j가 친구라면 distance[i][j] = 1, distance[j][i] = 1로 값을 업데이트
<div align="center">
<img src="https://github.com/user-attachments/assets/d8128e37-37b4-455e-808c-bbf2447a1ec1">
</div>

   - 다음 점화식을 이용해 플로이드-워셜 알고리즘을 수행해 3중 for문으로 모든 중간 경로 탐색
<div align="center">
<img src="https://github.com/user-attachments/assets/447375cb-b3b5-4c51-81b3-263239c18567">
</div>

   - 케빈 베이컨의 수(각 행의 합)을 비교해 가장 작은 수가 나온 행 번호를 정답으로 출력 : 같은 수가 있을 때 더 작은 행 번호를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/2646fac6-dcc6-4632-aab7-e6445018c4fc">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/28642d8e-db7a-49a0-80ee-171da1403c66">
</div>

7. 코드
```java
package graph.floydwarshall;

import java.io.*;
import java.util.StringTokenizer;

public class KevinBeaconSixStepPrinciple {
    private static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    private static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    public static int N, M;
    public static int distance[][];

    public static void main(String[] args) throws IOException {
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        distance = new int[N + 1][N + 1];

        // 인접 행렬 초기화
        for (int i = 1; i <= N; i++) {
            for (int j = 1; j <= N; j++) {
                if(i == j) {
                    distance[i][j] = 0;
                } else {
                    distance[i][j] = 10000001; // 충분히 큰 수로 저장
                }
            }
        }

        // 친구 관계이므로 양방향 가중치를 1로 저장
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());

            int s = Integer.parseInt(st.nextToken());
            int e = Integer.parseInt(st.nextToken());

            distance[s][e] = 1;
            distance[e][s] = 1;
        }

        // 플로이드-워셜 알고리즘 수행
        for(int k = 1; k <= N; k++) {
            for(int i = 1; i <= N; i++) {
                for(int j = 1; j <= N; j++) {
                    if (distance[i][j] > distance[i][k] + distance[k][j]) {
                        distance[i][j] = distance[i][k] + distance[k][j];
                    }
                }
            }
        }

        int Min = Integer.MAX_VALUE;
        int Answer = -1;

        for (int i = 1; i <= N; i++) {
            int temp = 0;

            for(int j = 1; j <= N; j++) {
                temp = temp + distance[i][j];
            }

            if(Min > temp) { // 가장 작은 케빈 베이컨 수를 지니고 있는 i를 찾기
                Min = temp;
                Answer = i;
            }
        }

        System.out.println(Answer);
    }
}
```
