-----
### 문제 90 - 2 * N 타일 채우기 (문제 11726번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/bb01c16d-66ca-456a-89e4-397ffb071e35">
</div>

2. 입력 : 1번째 줄에 N이 주어짐 (1 ≤ N ≤ 1,000)
3. 출력 : 1번째 줄에 2 X N 크기의 직사각형을 채우는 방법의 수를 10,007로 나눈 나머지를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/a1f74fae-bb05-4250-a562-a50a242d6095">
</div>

4. 1단계 : 문제 분석하기
   - 문제 내용에 따라 2 X N 크기의 직사각형 1 X 2 또는 2 X 1 크기의 타일로 채우는 경우의 수를 구하는 점화식 D[N] 정의
     + 점화식을 정의할 때는 문제가 단순화되도록 가정하는 것이 중요
     + 1부터 N - 1 크기의 직사각형과 관련된 경우의 수를 모두 구해놓았다고 가정하고, 문제에 접근

   - 먼저 N보다 작은 길이의 모든 경우의 수가 구해져 있다고 가정했으므로, N 바로 직전에 구해야 하는 N - 1, N - 2에서 N의 길이를 만들기 위한 경우의 수
<div align="center">
<img src="https://github.com/user-attachments/assets/5f83ab70-7e2b-40fe-a66b-d1b317b5cc90">
</div>

   - 점화식 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/5baad5d5-e9f1-462b-9829-883be018c7fa">
</div>

5. 2단계
   - 점화식의 형태와 의미 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/738678a5-8914-437a-985a-368354707411">
</div>

   - 점화식 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/409425d4-acac-43a1-91f8-87254179805d">
</div>

   - 점화식으로 D 배열을 채운 후 D[N]의 값을 출력 : D 배열을 채울 때 마다 문제에서 주어진 값 (10,007)으로 % 연산을 수행
<div align="center">
<img src="https://github.com/user-attachments/assets/308704ab-e295-47cf-8839-6ee54915a173">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/7b9eb253-9bb5-40e6-9bd0-e7526e939369">
</div>

7. 코드
```java
package dynamicprogramming;

import java.util.Scanner;

public class Tile {
    public static long mod = 10007;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        long D[] = new long[1001];

        D[1] = 1; // 길이가 2 X 1일 때, 타일의 경우의 수
        D[2] = 2; // 길이가 2 X 2일 때, 타일의 경우의 수

        for (int i = 3; i <= N; i++) {
            D[i] = (D[i - 1] + D[i - 2]) % mod;
        }

        System.out.println(D[N]);
    }
}
```
