-----
### 문제 71 - 리프 노드의 개수 구하기 (문제 1068번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/af2983ad-a287-461e-a47f-6ec37c036f6">
</div>

2. 입력
   - 1번째 줄에 트리의 노드 개수 N이 주어짐 (N은 50보다 작거나 같은 자연수)
   - 2번쨰 줄에 0번 노드부터 N - 1번 노드까지 각 노드의 부모가 주어짐 : 만약 부모가 없다면 루트 노드 - 1이 주어짐
   - 3번쨰 줄에는 지울 노드의 번호가 주어짐

3. 출력 : 1번쨰 줄에 입력으로 주어진 트리에서 입력으로 주어진 노드를 지웠을 때 남아 있는 리프 노드의 개수를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/83d086cf-ff73-4968-b68d-a0a4f061120c">
</div>

4. 1단계 : 문제 분석하기
   - '리프 노드를 어떻게 제거하는가?'가 핵심
   - 리프 노드를 탐색하는 탐색 알고리즘을 수행하면서 제거하는 노드가 나왔을 때는 탐색을 종료하는 아이디어를 적용 : 실제 리프 노드를 제거하는 효과

5. 2단계
   - 인접 리스트로 트리 데이터를 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/1a43b42a-2391-4fa2-a2fe-41b6df70d196">
</div>

   - DFS 또는 BFS 탐색을 수행하면서 리프 노드의 개수를 셈 : 단, 제거 대상을 만났을 때는 그 아래 자식 노드들과 관련된 탐색은 중지 (이는 제거한 노드의 범위에서 리프 노드를 제거하는 효과)
<div align="center">
<img src="https://github.com/user-attachments/assets/44960202-b83a-4405-8001-841256397da4">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/f86e2f28-262f-4501-9193-897be591614b">
</div>

7. 코드
```java
package tree.tree;

import java.util.ArrayList;
import java.util.Scanner;

public class LeafNode {
    public static ArrayList<Integer>[] tree;
    public static boolean[] visited;
    public static int answer = 0;
    public static int deleteNode = 0;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        tree = new ArrayList[N];
        visited = new boolean[N];
        int root = 0;

        for (int i = 0; i < N; i++) {
            tree[i] = new ArrayList<>();
        }

        for (int i = 0; i < N; i++) {
            int p = sc.nextInt();

            if (p != -1) {
                tree[i].add(p);
                tree[p].add(i);
            } else {
                root = i;
            }
        }

        deleteNode = sc.nextInt();
        if(deleteNode == root) {
            System.out.println(0);
        } else {
            DFS(root);
            System.out.println(answer);
        }
    }

    public static void DFS(int number) {
        visited[number] = true;

        int cNode = 0;

        for (int i : tree[number]) {
            if(visited[i] == false && i != deleteNode) { // 삭제 노드면 탐색 중지
                cNode++;
                DFS(i);
            }
        }

        if(cNode == 0) {
            answer++; // 자식 노드가 없을 때 리프 노드로 간주하고 정닶값 증가
        }
    }
}
```
