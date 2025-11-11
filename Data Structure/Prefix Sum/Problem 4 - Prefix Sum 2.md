-----
### 문제 4번 - 구간 합 구하기 2 (11660번)
-----
1. 문제
```
N X N개의 수가 N X N 크기의 표에 채워져 있다. 표 안의 수 중 (X₁, Y₁)에서 (X₂, Y₂)까지의 합을 구하려 한다.
X는 행, Y는 열을 의미한다. 예를 들어, N = 4이고, 표가 다음과 같이 채워져 있을 때를 살펴보자.
(2, 2)에서 (3, 4)까지의 합을 구하면 3 + 4 + 5 + 4 + 5 + 6 = 27이고, (4, 4)에서 (4, 4)까지의 합을 구하면 7이다.
표에 채워져 있는 수와 합을 구하는 연산이 주어졌을 때 이를 처리하는 프로그램을 작성하시오.
```
<div align="center">
<img src="https://github.com/user-attachments/assets/0d0755ed-bb7c-4b66-a31c-91f9a73855f8">
</div>

2. 입력
   - 1번째 줄에 표의 크기 N과 합을 구해야 하는 횟수 M이 주어짐 (1 ≤ N ≤ 1024, 1 ≤ M ≤ 100,000)
   - 2번쨰 줄부터 N개의 줄에는 표에 채워져 있는 수가 1행부터 차례대로 주어짐
   - 다음 M개의 줄에는 4개의 정수 X₁, Y₁, X₂, Y₂가 주어지며, (X₁, Y₁)에서 (X₂, Y₂)의 합을 구해 출력해야 함
   - 표에 채워져 있는 수는 1,000보다 작거나 같은 자연수 (X₁ ≤ X₂ Y₁ ≤ Y₂)

3. 출력 : 총 M줄에 걸쳐 (X₁, Y₁)에서 (X₂, Y₂)까지 합을 구해 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/504a25bd-2d79-49c7-9a72-fad1ca0e6034">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/e3158c35-0dba-460f-9a34-502b6728e052">
</div>

4. 1단계 : 문제 분석하기
   - 먼저 질의의 개수가 100,000이므로, 이 문제 역시 질의마다 합을 구하면 안 되고, 구간 합 배열을 이용해야 함
   - 2차원 구간 합 배열
<div align="center">
<img src="https://github.com/user-attachments/assets/d9a46607-2ac0-4919-ac1a-4f20c7f9f5fa">
</div>

5. 2단계
   - 2차원 구간 합 배열의 1행, 1열부터 구하기 : 구간 합 배열 1행, 1열은 다음과 같이 구할 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/6580da1a-a53d-490a-99f9-03e1f5315120">
</div>

   - 이를 통해 나머지 2차원 구간 합 배열을 채우기
<div align="center">
<img src="https://github.com/user-attachments/assets/561fe51d-19b5-447b-97ca-22e33905cf4f">
</div>

   - 구간 합 배열을 이용하기 전 질의에 대한 답을 도출하기 위한 과정을 원본 배열과 함께 확인
<div align="center">
<img src="https://github.com/user-attachments/assets/dcc5b3a4-88a0-407f-aba5-e367b9f3899d">
</div>

   - 예를 들어 질의가 2 2 3 4라면 (3, 4) 구간 합에서 (1, 4) 구간 합, (3, 1) 구간 합을 뺀 다음, 중복하여 뺀 (1, 1) 구간 합을 더하기
<div align="center">
<img src="https://github.com/user-attachments/assets/285d151c-222e-4a91-80ae-27a8c96d14cf">
</div>

   - 질의에 대한 답을 구하는 공식
<div align="center">
<img src="https://github.com/user-attachments/assets/91ad6da3-aef7-498e-98d0-ea77cc5cf74d">
</div>

6. 3단계 : 슈도코드 작성하기
<div align="center">
<img src="https://github.com/user-attachments/assets/13f91aa7-0c32-4f5e-a940-853719aa3740">
</div>

7. 코드
```java
package dataStructure.prefixsum;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class PrefixSum2 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int A[][] = new int[N + 1][N + 1];

        for (int i = 1; i <= N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 1; j <= N; j++) {
                A[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        int D[][] = new int[N + 1][N + 1];
        for (int i = 1; i <= N; i++) {
            for (int j = 1; j <= N; j++) {
                // 구간 합 구하기
                D[i][j] = D[i][j - 1] + D[i - 1][j] - D[i - 1][j - 1] + A[i][j];
            }
        }

        for (int i = 1; i <= M; i++) {
            st = new StringTokenizer(br.readLine());

            int x1 = Integer.parseInt(st.nextToken());
            int y1 = Integer.parseInt(st.nextToken());
            int x2 = Integer.parseInt(st.nextToken());
            int y2 = Integer.parseInt(st.nextToken());

            // 구간 합 배열로 질의에 답변하기
            int result = D[x2][y2] - D[x1 - 1][y2] - D[x2][y1 - 1] + D[x1 - 1][y1 - 1];
            System.out.println(result);
        }
    }
}
```
