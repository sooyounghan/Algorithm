-----
### 문제 35 - 동전 개수의 최솟값 구하기 (문제 11047번)
-----
1. 문제
```
준규가 소유하고 있는 동전은 총 N종류이고, 각 동전의 개수는 충분히 많다.
동전을 적절히 사용해 그 가격의 합을 K로 만들려고 한다.
이 때 필요한 동전 개수의 최솟값을 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 N과 K(1 ≤ N ≤ 10, 1 ≤ K ≤ 100,000,000)
   - 2번쨰 줄부터 N개의 줄에 동전의 가격 A가 오른차순으로 주어짐 (1 ≤ $A_{i}$ ≤ 1,000,000, $A_{1}$ = 1, i ≥ 2일 때, $A_{i-1}$의 배수)

3. 출력 : 1번째 줄에 K원을 만드는 데 필요한 동전 개수의 최솟값
<div align="center">
<img src="https://github.com/user-attachments/assets/2cfa01ff-9de0-4a16-a0fb-91d30dd1e86b">
</div>

4. 1단계 : 문제 분석하기
   - 전형적인 그리디 알고리즘 문제로, 이 문제는 그리디 알고리즘으로 풀 수 있도록 뒤의 동전 가격 $A_{i}$가 앞에 나오는 동전 가격의 $A_{i-1}$의 배수가 된다는 조건을 부여
   - 즉, 동전을 최소로 사용하여 K를 만들기 위해서는 가장 큰 동전부터 차례대로 사용하면 됨

5. 2단계
   - 가격이 큰 동전부터 내림차순으로 K보다 가격이 작거나 같은 동전이 나올 때까지 탐색
<div align="center">
<img src="https://github.com/user-attachments/assets/37f19d0d-687b-4cee-a9f2-7eb65dd2c1ea">
</div>

   - 탐색을 멈춘 동전의 가격을 K로 나눠 몫은 동전 개수에 더하고, 나머지는 K값으로 갱신
<div align="center">
<img src="https://github.com/user-attachments/assets/3b16591a-a8b1-49f1-a320-5a148cade78b">
</div>

   - 과정 1 ~ 2를 나머지가 0이 될 때까지 반복
<div align="center">
<img src="https://github.com/user-attachments/assets/dd31886a-757a-4406-a338-221e8f10c84c">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/7fa98cec-8f02-4070-86c6-7ad75419a98d">
</div>

7. 코드
```java
package greedy;

import java.util.Scanner;

public class CoinNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int K = sc.nextInt();
        int[] A = new int[N];

        for (int i = 0; i < N; i++) {
            A[i] = sc.nextInt();
        }

        int count = 0;
        for (int i = N - 1; i >= 0; i--) {
            if(A[i] <= K) { // 현재 동전의 가치가 K보다 작거나 같으면 구성에 추가
                count += (K / A[i]);
                K = K % A[i]; // K를 현재 동전을 사용하고 남은 금액으로 계산
            }
        }
        System.out.println(count);
    }
}
```
