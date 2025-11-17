-----
### 문제 67 - 최소 신장 트리 구하기 (문제 1197번)
-----
1. 문제
```
최소 신장 트리(최소 스패닝 트리)는 주어진 그래프의 모든 노드들을 연결하느 부분 그래프 중 그 가중치의 합이 최소인 트리를 말한다.
그래프가 주어졌을 때, 그 그래프의 최소 신장 트리를 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 노드의 개수 V(1 ≤ V ≤ 10,000)와 엣지의 개수 E(1 ≤ E ≤ 100,000)가 주어짐
   - 다음 E개의 줄에는 각 엣지와 관련된 정보를 나타내는 세 정수 A, B, C가 주어짐 : 이는 A번 노드와 B번 노드가 가중치 C인 엣지로 연결되어 있다는 의미 (C는 음수일 수도 있으며, 절댓값이 1,000,000을 넘지 않음)
   - 그래프의 노드는 1번부터 V번까지 번호가 매겨져 있고, 임의의 두 노드 사이에 경로가 있음
   - 최소 신장 트리의 가중치가 -2,147,483,648보다 크거나 같고, 2,147,483,647보다 작거나 같은 데이터만 입력으로 주어짐

3. 출력 : 1번재 줄에 최소 신장 트리이 가중치를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/e1f05d30-53e9-4a24-aa96-8ec02d649182">
</div>

4. 1단계 : 문제 분석하기
   - 최소 신장 트리를 구하는 가장 기본적인 문제

5. 2단계
   - 엣지 리스트에 엣지 정보를 저장한 후, 부모 노드 데이터를 초기화
     + 최소 신장 트리는 엣지 중심의 알고리즘이므로 데이터를 엣지 리스트를 활용해 저장해야 함
     + 사이클 생성 여부 판단을 위한 유니온 파인드용 부모 노드도 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/b83bf61a-9c22-4cba-b188-54eecf48db1d">
</div>

   - 크루스칼 알고리즘을 수행 : 현재 미사용 엣지 중 가중치가 가장 작은 엣지를 선택하고, 이 엣지를 연결했을 때 사이클 발생 여부 판단 (사이클이 발생하면 생략하고, 발생하지 않으면 엣지값을 더함) (프림 알고리즘으로도 가능)
<div align="center">
<img src="https://github.com/user-attachments/assets/64e63174-e9d5-49b3-b136-6e98a9c72b1d">
</div>

   - 과정 2에서 엣지를 더한 횟수가 'V(노드 개수) - 1'이 될 떄까지 반복하고, 반복이 끝나면 엣지의 가중치를 모두 더한 값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/0f33c377-55b0-4ecb-9051-5c38014dbf2c">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/32380965-8cf2-4178-8db6-388ba6e947a9">
</div>

7. 코드
```java
package graph.mst;

import java.util.PriorityQueue;
import java.util.Scanner;

public class MST {
    public static int[] parent;
    public static PriorityQueue<pEdge> queue;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt(); // 노드 수
        int M = sc.nextInt(); // 엣지 수

        queue = new PriorityQueue<>(); // 자동 정렬을 위해 우선순위 큐 자료구조 선택
        parent = new int[N + 1];

        for (int i = 0; i < N; i++) {
            parent[i] = i;
        }

        for(int i = 0; i < M; i++) {
            int s = sc.nextInt();
            int e = sc.nextInt();
            int v = sc.nextInt();

            queue.add(new pEdge(s, e, v));
        }

        int useEdge = 0;
        int result = 0;

        while (useEdge < N - 1) {
            pEdge now = queue.poll();

            if(find(now.s) != find(now.e)) { // 같은 부모가 아니라면 연결해도 사이클이 발생하지 않음
                union(now.s, now.e);

                result = result + now.v;
                useEdge++;
            }
        }

        System.out.println(result);
    }

    public static void union(int a, int b) { // union 연산 : 대표 노드끼리 연결하기
        a = find(a);
        b = find(b);

        if(a != b) {
            parent[b] = a;
        }
    }

    public static int find(int a) { // find 연산
        if(a == parent[a]) {
            return a;
        } else {
            return parent[a] = find(parent[a]);
        }
    }
}

class pEdge implements Comparable<pEdge> {
    int s;
    int e;
    int v;

    public pEdge(int s, int e, int v) {
        this.s = s;
        this.e = e;
        this.v = v;
    }

    @Override
    public int compareTo(pEdge o) {
        // 가중치를 기준으로 오름차순 정렬을 하기 위해 compareTo() 함수 재정의
        return this.v - o.v;
    }
}
```
