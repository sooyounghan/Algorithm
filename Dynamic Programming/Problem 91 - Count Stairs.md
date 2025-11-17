-----
### 문제 91 - 계단 수 구하기 (문제 10844번)
-----
1. 문제
```
45,656이라는 수를 살펴보자.
이 수의 인접한 모든 자릿수의 차이는 1이다.
이를 '계단 수'라고 한다.

준이는 수의 길이가 N인 계단 수가 몇 개 있는지 궁금해졌다.
N이 주어질 때, 길이가 N인 계단 수가 총 몇 개 있는지 구하는 프로그램을 작성하시오. (0으로 시작할 수는 없다.)
```

2. 입력 : 1번째 줄에 N이 주어짐 (N은 1보다 크거나 같고, 100보다 작거나 같은 자연수)
3. 출력 : 1번째 줄에 정답을 1,000,000,000으로 나눈 나머지를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/df557eea-5f47-4f38-be87-a42215f1c73d">
</div>

4. 1단계 : 문제 분석하기
   - 만약 N번째 길이에서 5로 끝나는 계단 수가 있었을 때, 이 계단 수의 N - 1의 자리에 올 수 있는 수는, 5와 1차이가 나는 4와 6
   - 계단 수의 특성을 이용한 정의
<div align="center">
<img src="https://github.com/user-attachments/assets/7a680650-0174-4bcf-b57c-e8f48547488d">
</div>

5. 2단계
   - 각 자릿수에 0 ~ 9 사이의 값이 들어오므로 높이에 따라 다른 점화식 도출
     + 먼저 N에서 계단 높이가 0일 때, 계단 수가 되려면 N - 1에서는 높이가 1이어야 함
     + N에서 계단 높이가 9일 때, 계단 수가 되려면 N - 1에서는 높이가 8이 되어야 함
     + 나머지는 가운데 계단이므로 H + 1, H - 1 양쪽에서 계단 수를 만들 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/3bcc5a3a-b214-4512-8b3f-9d0edbdf2515">
</div>

   - D 배열의 값을 초기화
     + 각 높이에서 길이가 1인 계단 수는 모두 1가지이므로 1로 초기화
     + 단, 0으로 시작할 수 없으므로 이 때는 0으로 초기화
     + D 배열을 채울 때 마다 1,000,000,000으로 % 연산을 수행해야 함
<div align="center">
<img src="https://github.com/user-attachments/assets/847d5872-ab5e-4032-bd7f-8b63aec6b3e7">
</div>

   - D[N][0] ~ D[N][9]의 모든 값을 더한 값을 출력 : N = 2일 때, 각 자릿수의 값을 모두 더하면 N = 2 길이에서 만들 수 있는 모든 계단 수의 경우의 수 출력 가능 (% 연산도 실시해야 함)

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/df3f45ca-86ad-4460-984a-140ec9d4b305">
</div>

7. 코드
```java
package dynamicprogramming;

import java.util.Scanner;

public class CountStair {
    public static long mod = 1000000000;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        long[][] D = new long[N + 1][11];

        for (int i = 1; i <= 9; i++) { // 숫자가 0으로 시작할 수 없으므로 D[1][0]은 0으로 초기화
            D[1][i] = 1;
        }

        for (int i = 2; i <= N; i++) {
            D[i][0] = D[i - 1][1];
            D[i][9] = D[i - 1][8];

            for (int j = 1; j <= 8; j++) {
                D[i][j] = (D[i - 1][j - 1] + D[i - 1][j + 1]) % mod;
            }
        }

        long sum = 0;
        for (int i = 0; i < 10; i++) {
            sum = (sum + D[N][i]) % mod; // 정답값을 더할 때도 % 연산 수행
        }

        System.out.println(sum);
    }
}
```
