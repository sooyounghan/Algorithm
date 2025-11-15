-----
### 문제 70 - 트리의 부모 찾기 (문제 11725번)
-----
1. 문제
```
루트 없는 트리가 주어진다.

이 때, 트리의 루트를 1이라고 정했을 떄, 각 노드의 부모를 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 노드의 개수 N(2 ≤ N ≤ 100,000)
   - 2번쨰 줄부터 N - 1개의 줄에 트리 상에 연결된 두 노드가 주어짐

3. 출력 : 1번쨰 줄부터 N - 1개의 줄에 각 노드의 부모 노드 번호와 2번 노드를 순서대로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/3946b32a-59ad-4b90-a4c8-90dd39078fe9">
</div>

4. 1단계 : 문제 분석하기
   - 주어지는 데이터가 단순히 연결되어 있는 두 노드를 알려주는 것이므로 데이터를 저장할 때 양방향 엣지로 간주하고 저장
   - 인접 리스트 자료 구조를 사용하면 간편하게 데이터를 저장할 수 있음
   - 트리의 루트가 1이라고 지정되었으므로, 1번 노드부터 DFS로 탐색하면서 부모 노드를 찾아주면 문제 해결 가능

5. 2단계
   - 인접 리스트로 트리 데이터 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/04de0d5f-dd40-4f97-968a-4c196f153f55">
</div>

   - DFS 탐색을 수행 : 수행할 때는 부모 노드의 값을 정답 배열에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/eb01c26d-dcc2-41e5-9dd8-ce8047eb152f">
</div>

   - 정답 배열의 2번 인덱스부터 값을 차례대로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/48198faa-c235-4f4f-895c-bc98c839f636">
</div>

   - 트리는 그래프 자료구조 중 하나의 형태이므로 그래프 구현 방식도 가능하며, 탐색 역시 그래프 탐색 알고리즘을 사용할 수 있음

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/f4fd5c9d-5310-4a47-ad76-0b373778c6cc">
</div>

7. 코드
```java
package tree.tree;

import java.util.ArrayList;
import java.util.Scanner;

public class ParentTree {
    public static int N;
    public static boolean[] visited;
    public static ArrayList<Integer> tree[];
    public static int answer[];

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        N = sc.nextInt();
        visited = new boolean[N + 1];
        tree = new ArrayList[N + 1];
        answer = new int[N + 1];

        for (int i = 0; i < tree.length; i++) {
            tree[i] = new ArrayList<>();
        }

        for (int i = 1; i < N; i++) {
            int n1 = sc.nextInt();
            int n2 = sc.nextInt();

            tree[n1].add(n2);
            tree[n2].add(n1);
        }

        DFS(1); // 부모 노드부터 DFS 시작

        for (int i = 2; i <= N; i++) {
            System.out.println(answer[i]);
        }
    }

    public static void DFS(int number) {
        visited[number] = true;

        for (int i : tree[number]) {
            if(!visited[i]) {
                answer[i] = number; // DFS를 탐색하면서 부모 노드를 정답 배열에 저장
                DFS(i);
            }
        }
    }
}
```
