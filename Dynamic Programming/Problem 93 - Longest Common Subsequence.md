-----
### 문제 93 - 최장 공통 부분 수열 찾기 (문제 9252번)
-----
1. 문제
```
LCS(Longest Common Subsequence, 최장 공통 부분 수열) 문제는 두 수열이 주어졌을 때, 모두의 부분 수열이 되는 수열 중 가장 긴 것을 찾는 문제다.

예를 들어, ACAYKP와 CPACK의 LCS는 ACAK이다.
```

2. 입력
   - 1번째 줄과 2번째 줄에 두 문자열이 주어짐
   - 문자열은 알파벳 대문자로만 이뤄져 있으며, 최대 1,000 글자로 이뤄져 있음

3. 출력
   - 1번째 줄에 입력으로 주어진 두 문자열의 LCS의 길이가 주어짐
   - 2번째 줄에 LCS를 출력 : LCS가 여러 가지일 때는 아무거나 출력하고, LCS의 길이가 0일 때는 2번째 줄을 출력하지 않음
<div align="center">
<img src="https://github.com/user-attachments/assets/d23cbe8e-8023-4222-8e4e-a5b843bad42b">
</div>

4. 1단계 : 문제 분석하기
   - LCS는 문자열을 이용한 대표적인 동적 계획법 문제 : 문제를 풀기 위해 각 문자열을 축으로 하는 2차원 배열 생성
<div align="center">
<img src="https://github.com/user-attachments/assets/a81f4574-d965-499c-b2af-d2e32019794d">
</div>

   - 이렇게 구성한 2차원 배열이 점화식 배열 : 배열에 저장하는 값은 각 위치 인덱스를 마지막 문자로 하는 두 문자열의 최장 공통 수열 길이

5. 2단계
   - LCS 점화식을 이용해 값을 채움 : 특정 자리가 가리키는 행과 열의 문자열값을 비교해 배열의 대각선 왼쪽 위의 값에 1을 더한 값
<div align="center">
<img src="https://github.com/user-attachments/assets/393c16ab-5d73-422b-8d05-dddd4c1d24fe">
</div>

   - 비교한 값이 다르면 배열의 왼쪽과 위쪽 값 중 큰 값을 선택해 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/d8ce61b9-2770-464e-ac2a-eb4dbbccf7e4">
</div>

   - 위에서 세운 점화식을 이용해 배열을 채우면, 두 문자열의 최장 공통 부분 수열 길이는 4
<div align="center">
<img src="https://github.com/user-attachments/assets/8d58aa44-eb15-4ec8-8b5e-352adfdec1ce">
</div>

   - LCS 정답을 출력
     + 먼저 마지막부터 탐색을 수행하고, 해당 자리에 있는 인덱스 문자열 값을 비교해 같으면 최장 공통 부분 수열에 해당하는 문자열을 기록하고, 왼쪽 대각선으로 이동
     + 비교한 값이 다르면 배열의 왼쪽과 위의 값 중 큰 값으로 이동
<div align="center">
<img src="https://github.com/user-attachments/assets/9a32b489-7551-474e-a622-3544232c96c5">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/6a43379f-4ac0-43be-ab11-687c3368fec4">
</div>

7. 코드
```java
package dynamicprogramming;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;

public class LCS {
    private static long[][] DP;
    private static ArrayList<Character> Path;
    private static char[] A;
    private static char[] B;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        A = br.readLine().toCharArray();
        B = br.readLine().toCharArray();

        DP = new long[A.length + 1][B.length + 1];
        Path = new ArrayList<Character>();

        for (int i = 1; i <= A.length; i++) {
            for (int j = 1; j <= B.length; j++ ) {
                if (A[i - 1] == B[j - 1]) {
                    DP[i][j] = DP[i - 1][j - 1] + 1; // 같은 문자열일 때 왼쪽 대각선의 값 + 1
                } else {
                    DP[i][j] = Math.max(DP[i - 1][j], DP[i][j - 1]); // 다르면, 왼쪽과 위쪽 값 중 큰 수
                }
            }
        }

        System.out.println(DP[A.length][B.length]);

        getText(A.length, B.length);

        for (int i = Path.size() - 1; i >= 0; i--) {
            System.out.print(Path.get(i));
        }

        System.out.println();
    }

    // LCS 출력 함수
    private static void getText(int r, int c) {
        if(r == 0 || c == 0) return;

        if(A[r - 1] == B[c - 1]) { // 같으면, LCS에 기록하고 왼쪽 위로 이동
            Path.add(A[r - 1]);
            getText(r - 1, c - 1);
        } else {
            if(DP[r - 1][c] > DP[r][c - 1]) { // 다르면 왼쪽과 위쪽 중 큰 수로 이동
              getText(r - 1, c);
            } else {
              getText(r, c - 1);
            }
        }
    }
}
```
