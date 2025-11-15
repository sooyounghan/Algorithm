-----
### 문제 53 - 집합 표현하기 (문제 1717번)
----
1. 문제
```
초기에 {0}, {1}, {2}, ..., {n}이 각각 n + 1개의 집합을 이루고 있다.
여기에 합집합 연산과 두 원소가 같은 집합에 포함되어 있는지를 확인하는 연산을 수행하려고 한다.

집합을 표현하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 n(1 ≤ n ≤ 1,000,000), m(1 ≤ m ≤ 100,000)이 주어짐 (m은 입력으로 주어지는 연산의 개수)
   - 다음 m개의 줄에는 각각의 연산이 주어짐
   - 합집합은 0 a b의 형태로 입력이 주어짐 : 이는 a가 포함되어 있는 집합과 b가 포함되어 있는 집합을 합친다는 의미
   - 두 우너소가 같은 집합에 포함되어 있는지 확인하는 연산은 1 a b의 형태로 입력이 주어짐
   - 이는 a와 b가 같은 집합에 포함되어 있는지를 확인하는 연산
   - a와 b는 n 이하 자연수 또는 0이고, 서로 같을 수 있음

3. 출력 : 1로 시작하는 입력에 1줄에 1개씩 YES 또는 NO로 결과를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/1a0182c8-4bce-46a9-9481-3d53316a6330">
</div>

4. 1단계 : 문제 분석하기
   - 최대 원소의 개수 1,000,000과 질의 개수 100,000이 큰 편이므로 경로 압축이 필요한 전형적인 유니온 파인드 문제

5. 2단계
   - 처음에는 노드가 연결되어 있지 않으므로 각 노드의 대표 노드는 자기 자신 : 각 노드의 값을 자기 인덱스 값으로 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/1a454959-acd6-4c5e-a7f7-c87fc4939404">
</div>

   - find 연산으로 특정 노드의 대표 노드를 찾고, union 연산으로 2개의 노드를 이용해 각 대표 노드를 찾아 연결하며, 질의한 값에 따라 결과 변환
<div align="center">
<img src="https://github.com/user-attachments/assets/7164dd5e-31c3-4efc-87f1-22a8417319cc">
</div>

   - 유니온 파인드에서 자주 실수하는 부분
     + find 연산을 수행할 때 재귀 함수에서 나오면서 탐색한 모든 노드의 대표 노드값을 이번 연산에서 발견한 대표 노드로 변경하는 부분
     + union 연산에서 선택된 노드끼리 연결하는 것이 아닌 선택된 노드의 대표 노드끼리 연결하는 부분
     + 두 가지가 가장 많이 실수하는 부분

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/379f9ebb-55a9-4d63-9cbd-4c0755bbd930">
</div>

7. 코드
```java
package graph.unionfind;

import java.util.Scanner;

public class SetExpression {
    public static int[] parent;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int M = sc.nextInt();
        parent = new int[N + 1];

        for (int i = 0; i <= N; i++) { // 대표 노드를 자기 자신으로 초기화
            parent[i] = i;
        }

        for (int i = 0; i < M; i++) {
            int question = sc.nextInt();
            int a = sc.nextInt();
            int b = sc.nextInt();

            if (question == 0) { // 집합 합치기
                union(a, b);
            } else { // 같은 집합 원소인지 확인
                if(checkSame(a, b)) {
                    System.out.println("YES");
                } else {
                    System.out.println("NO");
                }
            }
        }
    }

    public static void union(int a, int b) { // union 연산 : 대표 노드끼리 연결
        a = find(a);
        b = find(b);
    }

    public static int find(int a) { // find 연산
        if (a == parent[a]) {
            return a;
        } else {
            return parent[a] = find(parent[a]); // 재귀 함수 형태로 구현 : 경로 압축 부분
        }
    }

    public static boolean checkSame(int a, int b) { // 두 원소가 같은 집합인지 ghkrdls
        a = find(a);
        b = find(b);

        if(a == b) {
            return true;
        } else {
            return false;
        }
    }
}
```
