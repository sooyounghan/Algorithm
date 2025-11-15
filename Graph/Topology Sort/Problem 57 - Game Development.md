-----
### 문제 57 - 게임 개발하기 (문제 1516번)
-----
1. 문제
```
숌 회사에서 이번에 새로운 전략 시뮬레이션 게임 세준크래프트를 개발하기로 했다.
핵심적인 부분은 개발이 끝난 상태고, 종족별 균형과 전체 게임 시간 등을 조절하는 부분만 남아 있었다.
게임 플레이에 들어가는 시간은 상황에 따라 다를 수 있기 때문에 모든 건물을 짓는데 걸리는 최소의 시강늘 이용해 근사하기로 했다.

물론, 어떤 건물을 짓기 위해서는 다른 건물을 먼저 지어야 할 수도 있으므로 문제가 단순하지 않다.
예를 들면, 스타크레프트에서 벙커를 짓기 위해서는 배럭을 먼저 지어야 하므로 배럭을 먼저 지은 후 벙커를 지어야 한다.
여러 개의 건물을 동시에 지을 수 있다.

편의상 자원은 무한히 많고, 건물을 짓는 명령을 내리기까지 시간이 걸리지 않는다고 가정해보자.
N개의 건물을 지을 때 각 건물을 짓기 위해 필요한 최소 시간을 출력하시오.
```

2. 입력
   - 1번쨰 줄에 건물의 종류 수 (1 ≤ N ≤ 500)
   - 그 다음 N개의 줄에는 각 건물을 짓는데 걸리는 시간과 그 건물을 짓기 위해 먼저 지어야 하는 건물들의 번호가 주어짐
     + 건물의 번호는 1부터 N까지로 하고, 각 줄은 -1로 끝난다고 가정
     + 각 건물을 짓는 데 걸리는 시간은 100,000보다 작거나 같은 자연수

3. 출력 : N개의 각 건물이 완성되기까지 최소 시간을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/cc3a6d30-7997-414b-8731-4e75d70b0193">
</div>

4. 1단계 : 문제 분석하기
   - 이 문제를 풀기 위해서는 '어떤 건물을 짓기 위해 먼저 지어야 하는 건물이 있을 수 있다'라는 것이 중요
   - 각 건물을 노드라고 생각하면, 그래프 형태에서 노드 순서를 정렬하는 알고리즘인 위상 정렬을 사용하는 문제
   - 건물의 수가 500, 시간 복잡도가 2초이므로 시간 제한 부담은 거의 없음

5. 2단계
   - 입력 데이터를 바탕으로 필요한 자료구조 초기화 : 인접 리스트로 그래프를 표현하고 건물의 생산 시간을 따로 저장
     + 진입 차수 배열은 [0, 1, 1, 2, 1], 정답 배열은 모두 0으로 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/37862e5a-9008-4313-b278-5d6e880e987b">
</div>

   - 위상 정렬을 실행하면서 각 건물을 짓는데 걸리는 최대 시간을 업데이트
   - 진입 차수 배열과 위상 정렬 배열, 정답 배열 업데이트 방법 : Math.max(현재 건물(노드)에 저장된 최대 시간, 이전 건물(노드)에 저장된 최대 시간 + 현재 건물(노드)의 생산 시간)
<div align="center">
<img src="https://github.com/user-attachments/assets/4ebf3d22-72f3-45b4-bf9f-1740f2511f9f">
</div>

   - 정답 배열에 자기 건물을 짓는 데 걸리는 시간을 더한 후 정답 배열을 차례대로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/694931d9-afdb-4d7c-acff-d72d9eef0795">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/36f297a1-ffae-4c22-b4cd-a9eeba555f15">
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

public class GameDevelopment {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());

        ArrayList<ArrayList<Integer>> A = new ArrayList<ArrayList<Integer>>();
        for (int i = 0; i <= N; i++) {
            A.add(new ArrayList<>());
        }

        int[] indegree = new int[N + 1]; // 진입 차수 배열
        int[] selfBuild = new int[N + 1]; // 자기 자신을 짓는 데 걸리는 시간
        for (int i = 1; i <= N; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            selfBuild[i] = Integer.parseInt(st.nextToken()); // 자신의 생성 시간 저장

            while(true) {
                int preTemp = Integer.parseInt(st.nextToken());
                if(preTemp == -1) {
                    break;
                }

                A.get(preTemp).add(i);
                indegree[i]++; // 진입 차수 배열 데이터 저장
            }
        }
        
        // 위상 정렬
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 1; i <= N; i++) {
            if(indegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        int[] result = new int[N + 1];
        while (!queue.isEmpty()) {
            int now = queue.poll();

            for (int next : A.get(now)) {
                indegree[next]--;
                
                // 시간 업데이트 하기
                result[next] = Math.max(result[next], result[now] + selfBuild[now]);

                if(indegree[next] == 0) {
                    queue.offer(next);
                }
            }
        }

        for (int i = 1; i <= N; i++) {
            System.out.println(result[i] + selfBuild[i]);
        }
    }
}
```
