-----
### 문제 65 - 경로 찾기 (문제 11403번)
-----
1. 문제
```
가중치 없는 방향 그래프 G가 주어졌을 때 모든 노드 (i, j)에 관해 i에서 j로 가는 경로가 있는지 여부를 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 노드의 개수(1 ≤ N ≤ 100)가 주어짐
   - 2번째 줄부터 N개 줄에 그래프의 인접 행렬이 주어짐
   - i번쨰 줄의 j번쨰 숫자가 1일 경우, i에서 j로 가는 엣지가 존재한다는 뜻이고, 0일 경우 없다는 뜻
   - i번째 줄의 i번쨰 숫자는 항상 0

3. 출력 : 총 N개의 줄에 걸쳐 문제의 정답을 인접 행렬 형식으로 출력 (노드 i에서 j로 가는 경로가 있으면 i번쨰 줄의 j번째 숫자를 1, 없으면 0을 출력)
<div align="center">
<img src="https://github.com/user-attachments/assets/e7d62542-524e-4e15-b5b6-b6385d75067a">
</div>

4. 1단계 : 문제 분석하기
   - 플로이드-워셜 알고리즘을 이해하고, 문제의 요구사항에 따라 적절하게 수행할 수 있는지 묻는 문제
   - 모든 노드 쌍에 관해 경로가 있는지 여부를 확인하는 방법은 플로이드-워셜 알고리즘을 수행해 결과 배열을 그대로 출력
   - 단, 최단 거리를 구하는 문제가 아니므로, 기존 플로이드-워셜 알고리즘에서 최단 거리를 업데이트 하는 부분만 수정

5. 2단계
   - 입력 데이터를 인접 행렬에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/e618a6b4-aaf2-40f0-b3a8-4295d8125485">
</div>

   - 변경된 플로이드-워셜 알고리즘을 수행 : S와 E가 모든 중간 경로(K) 중 1개라도 연결되어 있다면 S와 E는 연결 노드로 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/f2a97236-880f-4c6c-b55f-c5d774de1482">
</div>

   - 알고리즘으로 변경된 인접 행렬을 출력

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/edb63ed4-c1fe-4b2b-8be9-a249b4ba551f">
</div>

7. 코드
```java
package graph.floydwarshall;

import java.io.*;
import java.util.StringTokenizer;

public class FindRoute {
    private static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    private static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    public static int N;
    public static int distance[][];

    public static void main(String[] args) throws IOException {
        N = Integer.parseInt(br.readLine());

        distance = new int[N][N];

        for (int i = 0; i < N; i++) { // 입력되는 인접 행렬의 값을 그대로 저장하기
            StringTokenizer st = new StringTokenizer(br.readLine());

            for(int j = 0; j < N; j++) {
                int v = Integer.parseInt(st.nextToken());
                distance[i][j] = v;
            }
        }

        // 변형된 플로이드-워셜 알고리즘 수행
        for(int k = 0; k < N; k++) {
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    if (distance[i][k] == 1 && distance[k][j] == 1) {
                        distance[i][j] = 1;
                    }
                }
            }
        }

        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                System.out.print(distance[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```
