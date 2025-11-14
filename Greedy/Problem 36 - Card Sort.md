-----
### 문제 36 - 카드 정렬하기 (문제 1715번)
-----
1. 문제
```
정렬된 두 묶음의 숫자 카드가 있다. 각 묶음의 카드 개수가 A, B일 때 보통 두 묶음을 합쳐 1개로 만들려면 A + B번 비교해야 함
예를 들어, 20장의 숫자 카드 묶음과 30장의 숫자 카드 묶음을 합치려면 50번의 비교가 필요하다.

매우 많은 숫자 카드 묶음이 책상 위에 놓여있다고 가정해보자. 이들을 두 묶음씩 골라 서로 합쳐 나가면 고르는 순서에 따라 비교 횟수가 달라진다.
예를 들어 10장, 20장, 40장의 묶음이 있다면 10장과 20장을 합친 후 30장 묶음과 40장 묶음을 합치면 (10 + 20) + (30 + 40) = 100번의 비교가 필요하다.
그러나 10장과 40장을 합친 후 합친 50장 묶음과 20장을 합치면 (10 + 40) + (50 + 20) = 120번의 비교가 필요하므로 효율적이지 않다.

N개의 숫자 카드 묶음의 크기가 각각 주어질 때 최소한 몇 번의 비교가 필요한지 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 N이 주어짐 (1 ≤ N ≤ 100,000)
   - 그 다음 N개의 줄에 걸쳐 숫자 카드 묶음 각각의 크기가 주어짐
   - 숫자 카드 묶음의 크기는 1,000보다 작거나 같은 양의 정수

3. 출력 : 1번째 줄에 최소 비교 횟수를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/6217eb13-958a-44a0-a1a7-4d4955e6e0ff">
</div>

4. 1단계 : 문제 분석하기
   - 잘 생각하면 먼저 선택된 카드 묶음이 비교 횟수에 더 많은 영향을 미침
   - 따라서 카드 묶음의 카드의 개수가 작은 순서대로 먼저 합치는 것이 전체 비교 횟수를 줄일 수 있는 방법
   - 현재 데이터 중 가장 작은 카드 개수를 가진 묶음 2개를 뽑아야 하고, 이 2개를 기준으로 합친 새로운 카드 묶음을 다시 데이터에 넣고 정렬해야 함
   - 즉, 데이터의 삽입 / 삭제 / 정렬이 자주 일어난다는 뜻 : 따라서, 이 문제는 우선순위 큐를 이용해야 함

5. 2단계
   - 현재 카드의 개수가 가장 작은 묶음 2개를 선택해 합침
   - 합친 카드 묶음을 다시 전체 카드 묶음 속에 넣음
   - 두 과정을 카드 묶음이 1개만 남을 때 까지 반복
<div align="center">
<img src="https://github.com/user-attachments/assets/d1cb2445-a2af-4f20-92ae-c1d91936a8c7">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/cc7906b3-4f4a-42ea-80c4-9e4f109d81ad">
</div>

7. 코드
```java
package greedy;

import java.util.PriorityQueue;
import java.util.Scanner;

public class CardSort {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        for (int i = 0; i < N; i++) {
            int data = sc.nextInt();
            pq.add(data);
        }

        int data1 = 0;
        int data2 = 0;
        int sum = 0;

        while(pq.size() != 1) {
            data1 = pq.remove();
            data2 = pq.remove();

            sum += data1 + data2;
            pq.add(sum);
        }

        System.out.println(sum);
    }
}
```
