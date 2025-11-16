-----
### 문제 79 - 이항계수 구하기 1 (문제 11050번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/26ca689e-82b4-409b-86a1-bcd38c4819fe">
</div>

2. 입력 : 1번째 줄에 N과 K가 주어짐 (1 ≤ N ≤ 10, 0 ≤ K ≤ N)
3. 출력 : $\binom{N}{K}$를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/cceaef4e-3955-486b-a2e9-ceaa59c35ad5">
</div>

4. 1단계 : 문제 분석하기
   - 조합에서 가장 기본이 되는 문제

5. 2단계
   - N과 K의 값을 입력받고 DP 배열을 선언 (D[N + 1][N + 1])
   - 그리고 DP 배열의 값을 다음과 같이 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/f4e47ad6-12e9-4047-92ac-dd26ef1e2503">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/9497fe70-290e-4423-bd48-ee661460626e">
</div>

   - 점화식으로 DP 배열의 값을 채움
<div align="center">
<img src="https://github.com/user-attachments/assets/751da2d4-9087-4d8a-82cc-486b470267ed">
</div>

   - D[N][K]의 값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/9d8345b2-09a3-4f65-9cbc-39d643e906c7">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/2edadcd9-7f00-41ff-bafa-3fe2ea64ebc5">
</div>

7. 코드
```java
package combination;

import java.util.Scanner;

public class BinomialCoefficient {
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
            }
        }

        System.out.println(D[N][K]);
    }
}
```
