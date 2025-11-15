-----
### 문제 56 - 줄 세우기
-----
1. 문제
```
N명의 학생들을 키 순서대로 줄을 세우려고 한다.

각 학생의 키를 직접 재서 정렬하면 간단하겠지만, 마땅한 방법이 없어 두 학생의 키를 비교하는 방법을 사용하기로 했다.
그나마도 모든 학생을 비교해본 것이 아니라 일부 학생들의 키만을 비교해봤다.

일부 학생들의 키를 비교한 결과가 주어졌을 때 줄을 세우는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 N(1 ≤ N ≤ 32,000), M(1 ≤ M ≤ 100,000)이 주어짐 (M은 키를 비교한 횟수)
   - 그다음 M개의 줄에는 키를 비교한 두 학생의 번호 A, B가 주어짐 : 이는 학생 A가 학생 B 앞에 서야 한다는 의미
   - 학생들의 번호는 1번부터 N번

3. 출력 : 1번쨰 줄부터 앞에서부터 줄을 세운 결과를 출력 (답이 여러 가지일 경우, 아무거나 출력)
<div align="center">
<img src="https://github.com/user-attachments/assets/f6944e73-c3e1-4a25-bd99-591e38cc053a">
</div>

4. 1단계 : 문제 분석하기
   - 학생들을 노드로 생각하고, 키 순서 비교 데이터로 엣지를 만든다고 생각했을 때 노드의 순서를 도출하는 가장 기본적인 문제
   - 특히 답이 여러 개일 때, 아무것이나 출력해도 된다는 전제 : 위상 정렬 결과값이 항상 유일하지 않다는 알고리즘 전제와 동일

5. 2단계
   - 인접 리스트에 노드 데이터를 저장하고, 진입 차수 배열값을 업데이트
<div align="center">
<img src="https://github.com/user-attachments/assets/5bd6265b-5dee-49c7-a7b9-88d723ecaa74">
</div>

   - 위상 정렬 수행 과정
     + 진입 차수가 0인 노드를 큐에 저장
     + 큐에서 데이터를 poll해 해당 노드를 탐색 결과에 추가하고, 해당 노드가 가리키는 노드의 진입 차수를 1씩 감소
     + 감소했을 때 진입 차수가 0이 되는 노드를 큐에 offer
     + 큐가 빌 때까지 세 과정을 반복
<div align="center">
<img src="https://github.com/user-attachments/assets/14aa2751-07d5-4a36-8353-8a77e77e559a">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/2e7e16ba-d811-4c9d-a5b4-bf0a6aed1e45">
</div>

7. 코드
```java
package graph.topologysort;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class LineUp {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int M = sc.nextInt();

        ArrayList<ArrayList<Integer>> A = new ArrayList<ArrayList<Integer>>();

        for (int i = 0; i <= N; i++) {
            A.add(new ArrayList<>());
        }

        int[] inDegree = new int[N + 1]; // 진입 차수 배열

        for (int i = 0; i < M; i++) {
            int S = sc.nextInt();
            int E = sc.nextInt();

            A.get(S).add(E);
            inDegree[E]++; // 진입 차수 배열 데이터 저장
        }

        Queue<Integer> queue = new LinkedList<>(); // 위상 정렬 수행

        for(int i = 1; i <= N; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }

        while(!queue.isEmpty()) {
            int now = queue.poll();

            System.out.print(now + " ");

            for (Integer next : A.get(now)) {
                inDegree[next]--;
                if (inDegree[next] == 0) {
                    queue.offer(next);
                }
            }
        }
    }
}
```
