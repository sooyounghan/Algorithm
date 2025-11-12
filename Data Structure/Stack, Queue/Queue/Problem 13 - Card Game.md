-----
### 문제 13 - 카드 게임 (문제 2164번)
-----
1. 문제
```
N장의 카드가 있다. 각각의 카드는 차례로 1에서 N까지의 번호가 붙어 있으며, 1번 카드가 가장 위, N번 카드가 가장 아래에 놓인 상태로 놓여 있다.
이제 다음과 같은 동작을 카드가 1장 남을 대까지 반복한다.

먼저 가장 위에 있는 카드를 바닥에 버린다. 그 다음 가장 위에 있는 카드를 가장 아래에 있는 카드 밑으로 옮긴다.
예를 들어 N = 4일때를 생각해 보자. 카드는 가장 위에서부터 1, 2, 3, 4의 순서대로 놓여있다.
1을 버리면 2, 3, 4가 남는다. 여기서 2를 가장 아래로 옮기면 3, 4, 2가 된다.
3을 버리면 4, 2가 남고, 4를 밑으로 옮기면 순서가 2, 4가 된다.
마지막으로 2를 버리면 카드 4가 남는다.

N이 주어졌을 때, 가장 마지막에 남는 카드를 구하는 프로그램을 작성하시오.
```

2. 입력 : 1번째 줄에 N(1 ≤ N ≤ 500,000)이 주어짐
3. 출력 : 1번째 줄에 남는 카드의 번호를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/994400ff-de0e-4808-836f-8499494c98a1">
</div>

4. 1단계 : 문제 분석하기
   - 가장 위의 카드를 가장 아래에 있는 카드 밑으로 옮기는 동작은 큐의 선입선출 성질을 이용하면 쉽게 구현할 수 있음
   - 카드의 개수의 최대가 500,00이므로 시간 복잡도 제약도 크지 않음

5. 2단계
   - 문제 푸는 순서
     + poll을 수행하여 맨 앞의 카드를 버림
     + 위 과정에 이어 바로 add를 수행해 맨 앞의 카드를 가장 아래로 옮김
     + 큐의 크기가 1이 될 떄까지 두 과정을 반복한 후 큐에 남은 원소를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/1b0f7f66-a492-4e15-9687-ea449ed0ce52">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/06f3ee74-7532-4c24-8a1c-1de5394bb52d">
</div>

7. 코드
```java
package dataStructure.queue;

import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class CardGame {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        Queue<Integer> myQueue = new LinkedList<Integer>();

        int N = sc.nextInt();

        for (int i = 1; i <= N; i++) {
            myQueue.add(i); // 카드를 큐에 저장
        }

        while (myQueue.size() > 1) { // 카드가 1장 남을 때까지
            myQueue.poll(); // 맨 위의 카드 버리기
            myQueue.add(myQueue.poll()); // 맨 위의 카드를 가장 아래 카드 밑으로 이동하기
        }

        System.out.println(myQueue.poll());
    }
}
```
