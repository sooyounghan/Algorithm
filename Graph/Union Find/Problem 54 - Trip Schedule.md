-----
### 문제 54 - 여행 계획 짜기 (문제 1976번)
-----
1. 문제
```
동혁이는 친구들과 함께 여행을 가려고 한다.

한국에는 도시가 N개 있고, 임의의 두 도시 사이에 길이 있을 수도, 없을 수도 있다.
동혁이는 여행 계획이 주어졌을 때 이 계획대로 여행할 수 있는지를 알아보려 한다.
물론, 중간에 다른 도시를 경유해 여행할 수도 있다.

예를 들어, 도시가 5개 있고, A-B, B-C, A-D, B-D, E-A의 길이 있고, 동혁이의 여행 계획이 E, C, B, C, D라면 E-A-B-C-B-C-B-D라는 여행 경로를 이용해 계획대로 여행할 수 있다.
도시의 개수와 도시 간의 연결 여부가 주어져 있고, 동혁이의 여행 계획에 속한 도시들이 순서대로 주어졌을 때 계획대로 여행이 가능한지 판별하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 도시의 수 N이 주어짐 (N ≤ 200)
   - 2번쨰 줄에 여행 계획에 속한 도시들의 수 M이 주어짐 (M ≤ 1000)
   - 다음 N개의 줄에는 N개의 정수가 주어짐
     + i번째 줄의 j번쨰 수는 i번 도시와 j번 도시의 연결 정보를 의미
     + 1이면 연결된 것이고, 0이면 연결되지 않은 것
   - A와 B가 연결됐으면, B와 A도 연결되어있음
   - 마지막 줄에는 여행 계획이 주어짐
   - 도시의 번호는 1에서 N까지 차례대로 매겨져 있음

3. 출력 : 1번째 줄에 가능하면 YES, 불가능하면 NO를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/57feb5b9-fb69-49ac-a193-d29bec21b2e9">
</div>

4. 1단계 : 문제 분석하기
   - 도시의 연결 여부를 유니온 파인드 연산을 이용해 해결할 수 있음
   - 일반적으로 유니온 파인드는 그래프 영역에서 많이 활용되지만, 단독으로도 활용 가능
   - 이 문제에서는 도시 간 연결 데이터를 인접 행렬 형태로 주었기 때문에, 인접 행렬을 탐색하면서 연결될 때마다 union 연산을 수행하는 방식으로 문제 접근

5. 2단계
   - 도시와 여행 경로 데이터를 저장하고, 각 노드와 관련된 대표 노드 배열의 값을 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/556377bb-82bd-40d2-9f6a-12cdc9d7f1dc">
</div>

   - 도시 연결 정보가 저장된 인접 행렬을 탐색하면서 도시가 연결되어 있을 때, union 연산 수행
<div align="center">
<img src="https://github.com/user-attachments/assets/8fa2f310-96d8-4008-bed9-b5bd911f717d">
</div>

   - 여행 경로에 포함된 도시의 대표 노드가 모두 같은지 확인한 후 결과값 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/64105140-6f1c-491c-b6ef-60ac3c805547">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/b5889308-5e57-407a-9baf-1bc12b287392">
</div>

7. 코드
```java
package graph.unionfind;

import java.util.Scanner;

public class TripSchedule {
    public static int[] parent;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int M = sc.nextInt();
        int[][] dosi = new int[N + 1][N + 1];

        for(int i = 1; i <= N; i++) { // 도시 연결 데이터 저장
            for(int j = 1; j <= N; j++) {
                dosi[i][j] = sc.nextInt();
            }
        }

        int[] route = new int[M + 1];
        for(int i = 1; i <= M; i++) { // 여행 도시 정보 저장
            route[i] = sc.nextInt();
        }

        parent = new int[N + 1];
        for(int i = 1; i <= N; i++) { // 대표 노드를 자기 자신으로 초기화
            parent[i] = i;
        }

        for(int i = 1; i <= N; i++) { // 인접 행렬에서 도시가 연결되어 있으면 union 연산 실행
            for(int j = 1; j <= N; j++) {
                if(dosi[i][j] == 1) {
                    union(i, j);
                }
            }
        }

        // 여행 게획 도시들이 1개의 대표 도시로 연결되어있는지 확인
        int index = find(route[1]);
        for(int i = 2; i < route.length; i++) {
            if(index != find(route[i])) {
                System.out.println("NO");
                return;
            }
        }
        System.out.println("YES");
    }

    public static void union(int a, int b) { // union 연산 : 대표 노드끼리 연결
        a = find(a);
        b = find(b);
        if(a != b) {
            parent[b] = a;
        }
    }

    private static int find(int a) { // find 연산
        if(a == parent[a]) {
            return a;
        } else {
            return parent[a] = find(parent[a]); // 재귀함수 형태로 구현 : 경로 압축 부분
        }
    }
}
```
