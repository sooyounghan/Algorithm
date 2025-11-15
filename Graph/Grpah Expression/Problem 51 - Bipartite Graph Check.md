-----
### 문제 51 - 이분 그래프 판별하기 (문제 1707번)
-----
1. 문제
```
각 집합에 속한 노드끼리 서로 인접하지 않는 두 집합으로 그래프의 노드를 나눌 수 있을 때, 이 그래프를 '이분 그래프'라고 한다.
그래프가 입력으로 주어졌을 때, 이 그래프가 이분 그래프인지 여부를 판별하는 프로그램을 작성하시오.
```

2. 입력
   - 입력은 여러 개의 사례로 구성
   - 1번쨰 줄에 테스트 케이스의 개수 K(2 ≤ K ≤ 5)가 주어짐
     + 각 사례의 1번째 줄에 그래프의 노드 개수 V(1 ≤ V ≤ 20,000)와 에지 개수 E(1 ≤ E ≤ 200,000)가 빈칸을 두고 순서대로 주어짐
     + 각 노드에는 1부터 V까지 차례로 번호가 붙어 있음
   - 2번째 줄부터 E개의 줄에 걸쳐 에지와 관련된 정보가 주어지는데, 각 줄에 인접한 두 노드의 번호가 공백 문자를 사이에 두고 주어짐

3. 출력 : K개의 줄에 걸쳐 입력으로 주어진 그래프가 이분 그래프면 YES, 아니면 NO를 순서대로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/5d929c7d-4501-4505-9ee9-63beff0785cb">
</div>

4. 1단계 : 문제 분석하기
   - 노드의 집합을 2개로 나누는데, 인접한 노드끼리 같은 집합이 되지 않도록 적절하게 임의로 분할할 수 있다고 함
   - 트리의 경우 항상 이분 그래프가 된다는 것을 알 수 있음
     + 사이클이 발생하지 않으면 탐색을 하면서 다음 노드를 이번 노드와 다른 집합으로 지정하면 되기 때문임
     + 단, 사이클이 발생했을 때, 이런 이분 그래프가 불가능할 때가 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/1d6099b7-b2e2-4e1d-b226-c789d5d2528c">
</div>

   - 이 때는 3번 노드를 1번 집합으로 설정하면 1번 노드와 인접하면서 같은 집합에 속하게 되고, 2번 집합으로 설정하면 2번 노드와 인접하면서 같은 집합에 속하게 되어 이분 그래프가 불가능하게 됨
   - 기존의 탐색 메커니즘에서 탐색한 노드에 다시 접근하게 됐을 때, 현재 노드 집합과 같으면 이분 그래프가 불가능하다는 것으로 판별

5. 2단계
   - 입력된 그래프 데이터를 인접 리스트로 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/db9ae986-bdac-4142-870c-da3c2f486bce">
</div>

   - 모든 노드로 각각 DFS 탐색 알고리즘을 적용해 탐색을 수행 : DFS를 실행할 때 현재 노드에서 연결된 노드 중 이미 방문한 노드가 나와 같은 집합이면 이분 그래프가 아닌 것으로 판별
   - 실행 결과가 이분 그래프가 아니면, 이후 노드는 탐색하지 않음
   - 2개 집합을 각각 A, B로 표현
<div align="center">
<img src="https://github.com/user-attachments/assets/1c8011b4-9c59-49d2-a8ba-06e42bc13507">
</div>

   - 이분 그래프 여부를 정답으로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/8d37cb94-2f4a-4adf-98c4-4229fa94e38c">
</div>

   - 사례의 개수만큼 위 세 과정을 반복
   - 여기서 모든 노드로 DFS를 실행하는 이유는 그래프의 모든 노드가 이어져 있지 않고, 여러 개의 부분 그래프로 이뤄진 케이스가 존재할 수 있기 때문임

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/8716b44e-3ee9-4fc3-a306-9a38196e9595">
</div>

7. 코드
```java
package graph.expression;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;

public class BipartiteGraph {
    public static ArrayList<Integer>[] A;
    public static int[] check;
    public static boolean visited[];
    public static boolean isEven;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());

        for(int t = 0; t < N; t++) {
            String[] s = br.readLine().split(" ");

            int V = Integer.parseInt(s[0]);
            int E = Integer.parseInt(s[1]);

            A = new ArrayList[V + 1];
            visited = new boolean[V + 1];
            check = new int[V + 1];
            isEven = true;

            for(int i = 1; i <= V; i++) {
                A[i] = new ArrayList<Integer>();
            }

            for (int i = 0; i < E; i++) { // 인접 리스트로 그래프 저장
                s = br.readLine().split(" ");

                int start = Integer.parseInt(s[0]);
                int end = Integer.parseInt(s[1]);

                A[start].add(end);
                A[end].add(start);
            }

            // 주어진 그래프가 1개로 연결되어있다는 보장이 없으므로 모든 노드에서 수행
            for(int i = 1; i <= V; i++) {
                if(isEven) {
                    DFS(i);
                } else {
                    break;
                }
            }

            if (isEven) {
                System.out.println("YES");
            } else {
                System.out.println("NO");
            }

        }
    }

    public static void DFS(int node) { // DFS 구현하기
        visited[node] = true;

        for (int i : A[node]) {
            if(!visited[i]) {
                // 인접한 노드는 같은 집합이 아니므로 이전 노드와 다른 집합으로 처리
                check[i] = (check[node] + 1) % 2;
                DFS(i);
            } else if (check[node] == check[i]) { // 이미 방문한 노드가 현재 내 노드와 같은 집합이면 이분 그래프가 아님
                isEven = false;
            }
        }
    }
}
```
