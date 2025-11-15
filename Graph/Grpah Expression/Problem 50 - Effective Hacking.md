-----
### 문제 50 - 효율적으로 해킹하기 (문제 1325번)
-----
1. 문제
```
해커 김지만은 잘 알려진 어느 회사를 해킹하려고 한다. 이 회사는 신뢰하는 관계와 신뢰하지 않은 관계로 이루어진 N개의 컴퓨터가 있다.
A가 B를 신뢰할 경우 B를 해킹하면 A도 해킹할 수 있다. 이 회사의 컴퓨터의 신뢰하는 관계가 주어졌을 때, 한 번에 가장 많은
컴퓨터를 해킹할 수 있는 컴퓨터의 번호를 출력하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 N과 M이 주어짐 (N은 10,000보다 작거나 같은 자연수, M은 100,000보다 작거나 같은 자연수)
   - 2번쨰 줄부터 M개의 줄에 신뢰하는 관계가 'A B'와 같은 형식으로 주어지며, 'A가 B를 신뢰한다.'를 의미
   - 컴퓨터는 1번부터 N번까지 번호가 1개씩 매겨져 있음

3. 출력 : 1번째 줄에 김지만이 한 번에 가장 많은 컴퓨터를 해킹할 수 있는 컴퓨터의 번호를 오름차순으로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/d5fcf834-28c3-45c1-8291-7147f295474b">
</div>

4. 1단계 : 문제 분석하기
   - N과 M의 크기가 작은 편이므로 시간 복잡도와 관련된 제약은 크지 않은 편
   - 신뢰 관계가 A, B라고 했을 때, A가 B를 신뢰한다는 것이 중요하며, 또한 가장 많은 컴퓨터를 해킹할 수 있는 컴퓨터는 신뢰를 가장 많이 받는 컴퓨터
   - 그래프의 노드와 엣지를 기준으로 이해하면 A라는 노드에서 탐색 알고리즘으로 방문하는 노드가 B, C라고 하면, B, C는 A에게 신뢰받는 노드가 됨

5. 2단계
   - 인접 리스트로 컴퓨터와 신뢰 관계 데이터의 그래프를 표현
<div align="center">
<img src="https://github.com/user-attachments/assets/51da32f3-c0ae-42ea-ba7f-7fd602f002ec">
</div>

   - 모든 노드로 각각 BFS 탐색 알고리즘을 적용해 탐색을 수행 : 탐색을 수행하면서 탐색되는 노드들의 신뢰도를 증가시켜 줌
<div align="center">
<img src="https://github.com/user-attachments/assets/2805e870-3fb7-4087-a32c-1754680103f0">
</div>

   - 탐색 종료 후 신뢰도 배열을 탐색해 신뢰도의 최댓값을 Max값으로 지정하고, 신뢰도 배열을 다시 탐색하면서 Max값을 지니고 있는 노드를 오름차순으로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/b428bcb8-8d28-4dc0-a190-bb1e680b3b28">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/bc9d487f-eef3-43c3-938a-d58c14c3537c">
</div>

7. 코드
```java
package graph.expression;

import java.io.*;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class EffectiveHacking {
    public static int N, M;
    public static boolean visited[];
    public static int answer[];
    public static ArrayList<Integer> A[];

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        A = new ArrayList[N + 1];
        answer = new int[N + 1];

        for (int i = 1; i <= N; i++) {
            A[i] = new ArrayList<>();
        }

        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int S = Integer.parseInt(st.nextToken());
            int E = Integer.parseInt(st.nextToken());
            A[S].add(E);
        }

        for (int i = 1; i <= N; i++) {
            visited = new boolean[N + 1];
            BFS(i);
        }

        int maxVal = 0;

        for(int i = 1; i <= N; i++) {
            maxVal = Math.max(maxVal, answer[i]);
        }

        for (int i = 1; i <= N; i++) {
            if(answer[i] == maxVal) { // answer 배열에서 maxVal과 같은 값을 지닌 index가 정답
                System.out.print(i + " ");
            }
        }
    }

    public static void BFS(int index) {
        Queue<Integer> queue = new LinkedList<>();
        queue.add(index);
        visited[index] = true;

        while (!queue.isEmpty()) {
            int newNode = queue.poll();
            for (int i : A[newNode]) {
                if (visited[i] == false) {
                    visited[i] = true;
                    answer[i]++; // 신규 노드 인덱스의 정답 배열 값을 증가시키기
                    queue.add(i);
                }

            }
        }
    }
}
```
