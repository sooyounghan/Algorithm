-----
### 문제 86 - 선물 전달하기 (문제 1947번)
-----
1. 문제
```
이번 ACM-ICPC 대회에 참가한 모든 사람이 선물을 1개씩 준비했다.
모든 사람은 선물을 1개씩 받고, 자기의 선물을 자기가 받는 경우는 없다.

대회가 끝나고 난 후 각자 선물을 전달하려고 할 때, 선물을 나누는 경우의 수를 구하는 프로그램을 작성하시오.
```

2. 입력 : 1번쨰 줄에 ACM-ICPC 대회에 참가한 학생의 수 N(1 ≤ N ≤ 1,000,000)이 주어짐
3. 출력 : 경우의 수를 1,000,000,000으로 나눈 나머지를 1번쨰 줄에 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/45c41225-ec47-4b98-aea8-a68d56d9ed04">
</div>

4. 1단계 : 문제 분석하기
   - 조합에서 다루기는 한지만, 완전 순열이라는 개념의 문제
   - 완전 순열의 개념은 n개의 원소 집합에서 원소들을 재배열할 때, 이전과 같은 위치에 배치되는 원소가 1개도 없을 때를 말하는 것
   - 하지만 이 문제에서 필요한 것은 완전 순열의 개념이 아니라 문제에 주어진 조건에 따라 적절한 점화식을 도출해내는 것

5. 2단계
   - D[N] 배열의 의미 : N명일 때, 선물을 교환할 수 있는 모든 경우의 수
     + N명이 존재한다고 가정하고, A가 B라는 학생에게 선물을 줬다고 가정
     + 이 때 교환 방식은 2가지 방식만이 존재
       * B도 A에게 선물을 줬을 때 (양방향 교환) : N명 중 2명이 교환을 완료했으므로 남은 경우의 수는 D[N - 2]
       * B는 A가 아닌 다른 친구에게 선물을 전달할 때 (단방향 교환) : N명 중 B만 받은 선물이 정해진 상태이므로 남은 학생은 N - 1이며, 경우의 수는 D[N - 1]

   - 이는 A가 B라는 학생에게 선물을 준 것으로 가정하고, 경우의 수를 생각한 것이지만, 실제 A는 자기 자신이 아닌 N - 1명에게 선물을 전달할 수 있는데, 이를 이용해 도출해 낸 완전 순열 경우의 수
<div align="center">
<img src="https://github.com/user-attachments/assets/5e1cc97c-5803-4002-83e5-acb8bf3a7b3e">
</div>

   - D 배열 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/29dd8146-e97f-4949-beef-4acedd960d05">
</div>

   - 완전 순열 점화식으로 정답 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/3afa655f-9aca-48d0-94dc-bb7360dc63fe">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/177b5f14-08fd-4391-ae10-149f1632bb1a">
</div>

7. 코드
```java
package combination;

import java.util.Scanner;

public class DeliverGift {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        long mod = 1000000000;

        long D[] = new long[1000001];

        D[1] = 0;
        D[2] = 1;

        for (int i = 3; i <= N; i++) {
            D[i] = (i - 1) * (D[i - 1] + D[i - 2]) % mod; // 완전 순열 점화식
        }

        System.out.println(D[N]);
    }
}
```
