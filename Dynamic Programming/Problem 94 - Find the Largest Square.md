-----
### 문제 94 - 가장 큰 정사각형 찾기
-----
1. 문제
```
크기가 n X m 이고, 0과 1로만 이루어진 배열이 있다.
이 배열에서 1로 된 가장 큰 정사각형의 크기를 구하는 프로그램을 작성하시오.

예를 들어, 다음 그림과 같은 배열에서는 가운데에 있는 2 X 2 크기의 정사각형이 가장 크다.
```

2. 입력
   - 1번쨰 줄에 n, m(1 ≤ n, m ≤ 1,000)이 주어짐
   - 그 다음 n개의 줄에 m개의 숫자로 배열이 주어짐

3. 출력 : 1번째 줄에 가장 큰 정사각형의 넓이를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/6eaffe86-5a70-408b-9c4e-94e5f43bc154">
</div>

4. 1단계 : 문제 분석하기
   - 가장 큰 정사각형의 넓이는 구하는 것은 가장 큰 정사각형의 한 변의 길이를 구한다는 것과 동일
   - 따라서, 변의 길이를 다음과 같이 D[i][j]로 정의
<div align="center">
<img src="https://github.com/user-attachments/assets/d18afdd3-4458-41bf-b2e1-40f6182d0156">
</div>

   - 다음과 같이 D 배열을 채우는 방식으로 점화식 아이디어를 생각할 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/c9ae4589-9bed-48a7-a782-dbde869b4dcc">
</div>

   - 물음표 위치의 원래 값이 1일 경우, 이 위치에서 위, 왼쪽, 대각선 위에 있는 값 중 가장 작은 값에 1을 더한 값으로 변경하며, 원래 값이 0일 경우 그대로 둠

5. 2단계
   - D 배열의 값을 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/7cc40a26-2808-4aac-a899-8057b7f8946d">
</div>

   - 점화식을 이용해 D 배열을 새롭게 채움
<div align="center">
<img src="https://github.com/user-attachments/assets/a64b2030-fc15-4a8c-8213-62f4b538668f">
</div>

   - D 배열 중 가장 큰 값의 제곱이 최대 정사각형의 넓이 : D 배열의 최댓값을 제곱하여 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/d7257646-8de5-4c56-8234-e5b67226e8e8">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/89509a33-dea6-4b6c-867c-b1380e481d4f">
</div>

7. 코드
```java
package dynamicprogramming;

import java.util.Scanner;

public class BigestSquare {
    public static void main(String[] args) {
        long[][] D = new long[1001][1001];

        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int m = sc.nextInt();

        long max = 0;

        for (int i = 0; i < n; i++) {
            String mline = sc.next();
            for (int j = 0; j < m; j++) {
                D[i][j] = Long.parseLong(String.valueOf(mline.charAt(j)));

                if(D[i][j] == 1 && j > 0 && i > 0) {
                    D[i][j] = Math.min(D[i - 1][j - 1], Math.min(D[i - 1][j], D[i][j - 1])) + D[i][j];
                }

                if(max < D[i][j]) {
                    max = D[i][j];
                }
            }
        }
        System.out.println(max * max);
    }
}
```
