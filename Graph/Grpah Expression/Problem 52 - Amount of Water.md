-----
### 문제 52 - 물의 양 구하기 (문제 2251번)
-----
1. 문제
```
각각 부피가 A, B, C(1 ≤ A, B, C ≤ 200) 리터인 3개의 물통이 있다.
처음에는 앞의 두 물통은 비어 있고, 3번쨰 물통은 가득(C 리터)차 있다.

이제 어떤 물통에 들어 있는 물을 다른 물통에 쏟아부을 수 있는데, 이 때는 한 물통이 비거나 다른 한 물통이 가득찰 때까지 물을 부을 수 있다.
이 과정에서 손실되는 물은 없다고 가정한다.

이와 같은 과정을 거치다보면 3번쨰 물통(용량이 C인)에 담겨 있는 물의 양이 변할 수 있다.

1번째 물통(용량이 A인)이 비어있을 때 3번째 물통(용량이 C인)에 담겨 있을 수 있는 물의 양을 모두 구하는 프로그램을 작성하시오.
```

2. 입력 : 1번째 줄에 세 정수 A, B, C가 주어짐
3. 출력 : 1번째 줄에 공백으로 구분해 답을 출력하며, 각 용량은 오름차순 정렬
<div align="center">
<img src="https://github.com/user-attachments/assets/9f691e8f-03f9-4487-aa45-cf432d428eeb">
</div>

4. 1단계 : 문제 분석하기
   - 그래프 원리를 적용해 그래프를 역으로 그리는 방식
   - A, B, C의 특정 무게 상태를 1개의 노드로 가정하고, 조건에 따라 이 상태에서 변경할 수 있는 이후 묵 ㅔ상태가 에지로 이어진 인접한 노드라고 생각

5. 2단계
   - 처음에는 물통 A, B는 비어 있고, C는 꽉 차 있으므로 최초 출발 노드를(0, 0, 3번째 물통의 용량)으로 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/09ceba7d-d222-4bd0-9363-3f16be43e77b">
</div>

   - BFS를 수행
     + 노드에서 갈 수 있는 6가지 경우(A → B, A → C, B → A, B → C, C → A, C → B)에 관해 다음 노드로 정해 큐에 추가
       * A, B, C 무게가 동일한 노드에 방문한 이력이 있을 때는 큐에 추가하지 않음
     + 보내는 물통의 모든 용량을 받는 물통에 저장하고, 보내는 물통에는 0을 저장 : 단, 받는 물통이 넘칠 때는 초과하는 값만큼 보내는 물통에 남김
     + 큐에 추가하는 시점에 1번째 물통(A)의 무게가 0일 땡가 있으면, 3번째 물통(C)의 값을 정답 배열에 추가
<div align="center">
<img src="https://github.com/user-attachments/assets/73e9cbf3-7c8e-4f27-91ea-12da6c41c02c">
</div>

   - 정답 리스트를 오름차순 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/be1da718-9ac7-440b-a6fa-ae1640905d16">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/939edbba-ca2f-409d-b7b4-cff18831d366">
</div>

7. 코드
```java
package graph.expression;

import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class WaterAmount {
    // 6가지 이동 케이스를 표현하기 위한 배열
    public static int[] Sender = { 0, 0, 1, 1, 2, 2 };
    public static int[] Receiver = { 1, 2, 0, 2, 0, 1 };
    public static boolean visited[][]; // A, B의 무게가 있으면 C 무게가 고정되므로 2개만 체크
    public static boolean answer[];
    public static int now[];

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        now = new int[3]; // A, B, C 물의 양을 저장하는 배열
        now[0] = sc.nextInt();
        now[1] = sc.nextInt();
        now[2] = sc.nextInt();

        visited = new boolean[201][201];
        answer = new boolean[201];

        BFS();

        for (int i = 0; i < answer.length; i++) {
            if(answer[i]) {
                System.out.print(i + " ");
            }
        }
    }

    public static void BFS() {
        Queue<AB> queue = new LinkedList<>();

        queue.add(new AB(0, 0));
        visited[0][0] = true;
        answer[now[2]] = true;

        while(!queue.isEmpty()) {
            AB p = queue.poll();

            int A = p.A;
            int B = p.B;
            int C = now[2] - A - B; // C는 전체 물의 양에서 A와 B를 뺀 것

            // A → B, A → C, B → A, B → C, C → A, C → B
            for(int k = 0; k < 6; k++) {
                int[] next = { A, B, C };

                next[Receiver[k]] += next[Sender[k]];
                next[Sender[k]] = 0;

                if (next[Receiver[k]] > now[Receiver[k]]) { // 물이 넘칠 때
                    // 초과하는 만큼 다시 이전 물통에 넣어줌
                    next[Sender[k]] = next[Receiver[k]] - now[Receiver[k]];
                    next[Receiver[k]] = now[Receiver[k]]; // 대상 물통은 최대로 채워 줌
                }

                if(!visited[next[0]][next[1]]) { // A와 B의 물의 양을 이용해 방문 배열 체크
                    visited[next[0]][next[1]] = true;
                    queue.add(new AB(next[0], next[1]));

                    if(next[0] == 0) { // A의 물의 양이 0일 떄 C의 물의 양을 정답 변수에 저장
                        answer[next[2]] = true;
                    }
                }
            }
        }
    }
}

// AB 클래스 선언 : A와 B 값만 지니고 있으면, C를 유추할 수 있으므로 두 변수만 사용
class AB {
    int A;
    int B;

    public AB(int a, int b) {
        A = a;
        B = b;
    }
}
```
