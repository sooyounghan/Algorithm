-----
### 문제 25번 - 친구 관계 파악하기 (문제 13023번)
-----
1. 문제
```
BOJ 알고리즘 캠프에는 총 N명이 참가하고 있다. 사람들은 0번부터 N - 1번으로 번호가 매개져 있고, 일부 사람들은 친구다.
오늘은 다음과 같은 친구 관계를 가진 사람 A, B, C, D, E가 존재하는 구해보려고 한다
   - A는 B와 친구다.
   - B는 C와 친구다.
   - C는 D와 친구다.
   - D는 E와 친구다.

위와 같은 친구 관계가 존재하는지 여부를 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 사람의 수 N(5 ≤ N ≤ 2,000)과 친구 관계의 수 M(1 ≤ M ≤ 2,000)
   - 2번째 줄부터 M개의 줄에 정수 a와 b가 주어짐 : a와 b는 친구라는 뜻(0 ≤ a, b ≤ N - 1, a ≠ ㅠ)
   - 같은 친구 관계가 2번 이상 주어지지 않음

3. 출력 : 문제의 조건에 맞는 A, B, C, D, E가 존재할 때 1, 없으면 0을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/06879275-b394-4fdf-a895-0f5eefc365ec">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/ac67195a-3216-4786-a710-d84e2ee7f08f">
</div>

4. 1단계 : 문제 분석하기
   - N의 최대 범위가 2,000이므로 알고리즘의 시간 복잡도를 고려할 때 자유로움
   - 문제에서 요구하는 A, B, C, D, E의 관계는 재귀 함수 형태와 비슷 : 주어진 모든 노드에 DFS를 수행하고 재귀의 깊이가 5 이상(5개의 노드가 재귀 형태로 연결)이면 1, 아니라면 0을 출력
   - DFS의 시간 복잡도는 O(V + E)이므로 최대 4,000, 모든 노드를 진행했을 때 4,000 * 2,000 = 8,000,000이므로 DFS를 사용해도 제한 시간 내 문제 해결 가능

5. 2단계
   - 그래프 데이터를 인접 리스트로 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/09f66d38-39c8-4816-90eb-1013ba4d19d5">
</div>

   - 모든 노드에서 DFS를 수행 : 수행할 때 재귀 호출마다 깊이를 더함 (깊이가 5가 되면, 1을 출력하고 프로그램을 종료)
<div align="center">
<img src="https://github.com/user-attachments/assets/b1d330b2-e30c-432e-a17d-ac48cb752408">
</div>

   - 모든 노드를 돌아도 1이 출력되지 않았다면 0을 출력

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/f0e7e1f2-4ba8-4ba9-8c5b-bfba795b7133">
</div>

7. 코드
```java
package search.dfs;

import java.util.ArrayList;
import java.util.Scanner;

public class FriendRelationship {
    public static boolean visited[];
    public static ArrayList<Integer>[] A;
    public static boolean arrive;

    public static void main(String[] args) {
        int N; // 노드 개수
        int M; // 엣지 개수
        arrive = false;

        Scanner sc = new Scanner(System.in);

        N = sc.nextInt();
        M = sc.nextInt();
        A = new ArrayList[N];
        visited = new boolean[N];

        for (int i = 0; i < N; i++) {
            A[i] = new ArrayList<Integer>();
        }

        for (int i = 0; i < M; i++) {
            int S = sc.nextInt();
            int E = sc.nextInt();
            A[S].add(E);
            A[E].add(S);
        }

        for (int i = 0; i < N; i++) {
            DFS(i, 1); // Depth 1부터 시작

            if(arrive) {
                break;
            }
        }

        if(arrive) {
            System.out.println("1");
        } else {
            System.out.println("0");
        }
    }

    public static void DFS(int now, int depth) { // DFS 구현
        if(depth == 5 || arrive) { // Depth가 5가 되면 프로그램 종료
            arrive = true;
            return;
        }

        visited[now] = true;

        for (int i : A[now]) {
            if(!visited[i]) {
                DFS(i, depth + 1); // 재귀 호출이 될 때마다 depth를 1씩 증가
            }
        }

        visited[now] = false;
    }
}
```
