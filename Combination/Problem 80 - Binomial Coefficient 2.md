-----
### 문제 80 - 이항계수 구하기 2 (문제 11051번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/ea073dce-05ac-4d38-996e-fd6b938bff89">
</div>

2. 입력 : 1번쨰 줄에 N과 K가 주어짐 (1 ≤ N ≤ 1,000, O ≤ K ≤ N)
3. 출력 : $\binom{N}{K}$를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/80337bba-e17d-4c76-a687-3c80dea7002f">
</div>

4. 1단계 : 문제 분석하기
   - 이항계수 문제와 동일하지만, N의 범위가 커진 상태이고, 결과값을 10,007로 나눈 나머지를 출력하라는 요구사항
   - 모듈러 연산의 특성 : (A mod N + B mod N) mod N = (A + B) mod N
     + 모듈러 연산은 덧셈에 관해 각각 모듈러 연산하여 더한 값에 다시 모듈러 연산을 수행한 것과 두 수를 더한 후 수행한 것의 값이 동일
   - 이 문제에서 D 배열에 결과값이 나올 때마다 모듈러 연산을 수행하는 로직을 추가하면 해결 가능

5. 2단계
   - N과 K의 값을 입력받은 후 DP 배열 생성 (D[N + 1][N + 1]) 및 DP 배열의 값을 다음과 같이 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/a046ff4a-89db-4666-b9eb-b6baa172b0c7">
</div>

   - 점화식으로 DP 배열의 값을 채움 : 이 때 점화식의 값이 나올 때마다 MOD 연산 수행
<div align="center">
<img src="https://github.com/user-attachments/assets/a27b8aa6-fb89-4d19-9b53-190c8bac8b30">
</div>

   - D[N][K]의 값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/dcc377b6-a088-4fd9-94fd-f779f3e6a5a2">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/6dfc717b-6630-495a-95d0-edb04a22aa74">
</div>

7. 코드
```java
package combination;

import java.util.Scanner;

public class BinomialCoefficient2 {
    public static int N, K;
    public static int[][] D;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        N = sc.nextInt();
        K = sc.nextInt();
        D = new int[N + 1][N + 1];

        for (int i = 0; i <= N; i++) {
            D[i][1] = i;
            D[i][0] = 1;
            D[i][i] = 1;
        }

        for (int i = 2; i <= N; i++) {
            for (int j = 1; j < i; j++) {
                D[i][j] = D[i - 1][j] + D[i - 1][j - 1]; // 조합 기본 점화식
                D[i][j] = D[i][j] % 10007;
            }
        }

        System.out.println(D[N][K]);
    }
}
```
