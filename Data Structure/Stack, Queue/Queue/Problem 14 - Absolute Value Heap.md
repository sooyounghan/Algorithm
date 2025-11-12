-----
### 문제 14 - 절댓값 힙 구현하기 (문제 11286번)
-----
1. 문제
```
절댓값 힙은 다음과 같은 연산을 지원하는 자료구조이다.
   1. 배열에 정수 x(x ≠ 0)를 넣는다.
   2. 배열에서 절댓값이 가장 작은 값을 출력한 후 그 값을 배열에서 제거한다.
      절댓값이 가장 작은 값이 여러 개일 경우에는 그 중 가장 작은 수를 출력하고, 그 값을 배열에서 제거한다.
프로그램은 처음 비어 있는 배열에서 시작한다. 절댓값 힙을 구현하시오.
```

2. 입력
   - 1번째 줄의 연산의 개수 N(1 ≤ N ≤ 100,000)이 주어짐
   - 다음 N개의 줄에는 연산과 관련된 정보를 나타내는 정수 x가 주어짐
   - 만약 x가 0이 아니라면 배열에 x라는 값을 추가하고, x가 0이라면 배열에서 절댓값이 가장 작은 값을 출력하고, 그 값을 배열에서 제거한다.
   - 입력되는 정수는 $-2^{31}$보다 크고, $2^{31}$보다 작음

3. 출력 : 입력에서 0이 주어진 횟수만큼 답을 출력하되, 만약 배열이 비어 있는데 절댓값이 가장 작은 값을 출력하라고 할 때는 0을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/7ed38001-df48-4ef7-ab1d-bbdea73d5eaa">
</div>

4. 1단계 : 문제 분석하기
   - N의 최대 범위가 100,000으로 O(n log n) 시간 복잡도를 가진 알고리즘으로 해결 가능
   - 데이터가 삽입될 때마다 절댓값과 관련된 정렬이 필요하므로 우선순위 큐로 문제를 쉽게 해결 가능
   - 단, 이 문제는 절댓값 정렬이 필요하므로 우선순위 큐의 정렬 기준을 직접 정의해야 함 : 예제에서 절댓값이 같을 때는 음수를 우선하여 출력해야 함

5. 2단계
   - 문제 푸는 순서
     + x = 0 일 때 : 큐가 비어 있을 때는 0을 출력하고, 비어 있지 않을 때는 절댓값이 최소인 값을 출력 (단, 절댓값이 같으면 음수를 우선하여 출력)
     + x ≠ 0 일 때 : add로 큐에 새로운 값을 추가하고 우선순위 큐 정렬 기준으로 자동 정렬
<div align="center">
<img src="https://github.com/user-attachments/assets/a8da6d5a-ae45-4bda-bcf8-d0cdeac84c90">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/e9e11ffc-efee-4146-9cbf-159fab21c6e3">
</div>

7. 코드
```java
package dataStructure.prorityqueue;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.PriorityQueue;

public class AbsoluteHeap {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());
        PriorityQueue<Integer> myQueue = new PriorityQueue<>((o1, o2) -> {
            int first_abs = Math.abs(o1);
            int second_abs = Math.abs(o2);

            if(first_abs == second_abs) {
                return o1 > o2 ? 1 : -1; // 절댓값이 같으면 음수 우선 정렬
            } else {
                return first_abs - second_abs; // 절댓값을 기준으로 정렬
            }
        });

        for (int i = 0; i < N; i++) {
            int request = Integer.parseInt(br.readLine());

            if(request == 0) {
                if(myQueue.isEmpty()) {
                    System.out.println("0");
                } else {
                    System.out.println(myQueue.poll());
                }
            } else {
                myQueue.add(request);
            }
        }
    }
}
```
