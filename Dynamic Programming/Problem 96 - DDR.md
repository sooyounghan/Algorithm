-----
### 문제 96 - DDR을 해보자 (문제 2342번)
-----
1. 문제
```
승환이는 요즘 댄스 레볼루션, 즉, DDR이라는 게임에 빠져 살고 있다.
DDR은 다음 그림과 같은 모양의 발판이 있고, 주어진 스템에 맞춰 나가는 게임이다.
발판은 1개의 중점을 기준으로 위, 아래, 왼쪽, 오른쪽으로 연결돼 있다.
편의상 중점을 0, 위를 1, 왼쪽을 2, 아래를 3, 오른쪽을 4라고 정하자
```
<div align="center">
<img src="https://github.com/user-attachments/assets/b209aa9c-fb8b-470a-b0c1-ef365c32eb39">
</div>

```
게임의 규칙은 이렇다.

처음에 게이머는 두 발을 중앙에 모으고 있다. (그림에서 0의 위치)
게임이 시작하면 지시에 따라 왼쪽 또는 오른쪽에 발을 움직인다.
두 발을 동시에 움직이면 안 되고, 게임을 시작했을 떄를 제외하고 두 발이 같은 지점에 있어서는 안 된다.
예를 들어, 한 발이 1의 위치에 있고, 다른 한 발이 3의 위치에 있을 때, 3을 연속으로 눌러야 한다면, 3의 위치에 있는 발로 계속 눌러야 한다.

오랫동안 DDR을 해 온 승환이는 발이 움직이는 위치에 따라 드는 힘이 다르다는 것을 알게 됐다.
예를 들어, 중앙에 있던 발이 다른 지점으로 움직일 떄는 2의 힘을 사용한다.
다른 지점에서 인접한 지점으로 움직일 때는 3의 힘을 사용한다. 예를 들어, 2에서 1 또는 3으로 이동할 때는 3의 힘을 사용한다.
반대편의 힘을 움직일 때는 4의 힘을 사용한다.
같은 지점을 한 번 더 누르면 1의 힘을 사용한다.

만약 1 → 2 → 2 → 4 를 눌러야 한다고 가정해보자.
두 발은 처음에 (0, 0)에 위치하고 있으므로 (0, 0) → (0, 1) → (2, 1) → (2, 4) 순으로 발을 움직여야 한다. 이러면 8의 힘을 사용한다.
8의 힘보다 더 적은 힘으로 1 → 2 → 2 → 4를 누를 수 있는 다른 방법은 없다.

이렇게 게임에서 주어진 발판 순서를 최소한의 힘으로 누르는 경로를 알려주는 프로그램을 작성하시오.
```

2. 입력
   - 입력은 지시 사항으로 이뤄림
     + 각각의 지시 사항은 1개의 수열로 이뤄짐
     + 각 수 열은 1, 2, 3, 4의 숫자들로 이뤄지고, 이 숫자들은 각각의 방향을 나타냄
     + 그리고 0은 수열의 마지막을 의미
   - 즉, 입력 파일의 마지막에는 0이 입력됨
   - 입력되는 수열의 길이는 100,000을 넘지 않음

3. 출력 : 1번째 줄에 모든 지시 사항을 만족하는 필요한 힘의 최솟값 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/853b341e-8e32-4842-b460-53b802152271">
</div>

4. 1단계 : 문제 분석하기
   - 아이디어는 수열의 최대 길이가 10만이므로 모든 경우의 수를 점화식으로 표현해보는 것
<div align="center">
<img src="https://github.com/user-attachments/assets/a0f3ee62-46d3-4863-a564-179ea7c13308">
</div>

   - 위와 같이 정의하면 직전 수열까지 구한 최솟값을 이용해 해당 값을 구할 수 있음
   - 예를 들어, 오른 다리가 2의 자리에 있었다가 현재 R자리로 이동했다면 D[N][L][R]의 최솟값 후보 중 하나로 D[N-1][L][2] + (2 → R로 이동한 힘)이 될 수 있음
   - 마찬가지로, 왼반을 움직일 때도 동일하게, 예를 들어, D[N-1][L'][R] + (L' : 직전 왼발의 위치 → L로 이동한 힘) 역시 D[N][L][R]의 최솟값 후보가 될 수 있음
   - 즉, 한 발만 움직여 D[N][L][R]의 위치를 만들 수 있는 모든 경우의 수를 비교해 최솟값을 이 위치에 저장하는 작업을 수행하면 해결 가능

5. 2단계
   - 점화식 D[N][L][R]을 구함 : mp[i][j]를 i에서 j로 이동하는 데 드는 힘이라고 가정
   - 바로 직전에 오른발을 움직일 때 점화식
<div align="center">
<img src="https://github.com/user-attachments/assets/ee5dfc07-34f5-415b-8553-53278deb54b0">
</div>

   - 바로 직전에 왼발을 움직일 때의 점화식
<div align="center">
<img src="https://github.com/user-attachments/assets/4c781ff8-2f32-414b-bfd0-7f641b6b2cb1">
</div>

   - 단, 이 점화식을 왼발과 오른발을 구분해 두 발로 만들 수 있는 모든 경우를 고려해 반복해야 함

   - 충분히 큰 수로 D 배열을 초기화하고, 점화식을 이용해 값 채움
<div align="center">
<img src="https://github.com/user-attachments/assets/f551a474-03f6-424d-b051-3f9b08976569">
</div>

   - 수열을 모두 수행한 후 최솟값 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/f706569a-37a1-458f-b380-96eafee125ff">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/4521d829-546f-4551-bdb4-622df1243d27">
</div>

7. 코드
```java
package dynamicprogramming;

import java.util.Scanner;

public class DDR {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // dp[N][L][R] = N개 수열을 수행했고, 왼쪽이 L / 오른쪽이 R에 있을 때 최소 누적 합
        int dp[][][] = new int[100001][5][5];

        // 한 발을 이동할 때 드는 힘을 미리 저장 (mp[1][2] : 1에서 2로 이동할 때 드는 힘)
        int mp[][] = { { 0, 2, 2, 2, 2 },
                       { 2, 1, 3, 4, 3 },
                       { 2, 3, 1, 3, 4 },
                       { 2, 4, 3, 1, 3 },
                       { 2, 3, 4, 3, 1 } };

        int n = 0, s = 1;

        for(int i = 0; i < 5; i++) {
            for(int j = 0; j < 5; j++) {
                for(int k = 0; k < 100001; k++) {
                    dp[k][i][j] = 100001 * 4; // 충분히 큰 수로 초기화
                }
            }
        }

        dp[0][0][0] = 0;

        while(true) {
            n = sc.nextInt();

            if (n == 0) { // 입력의 마지막이면 종료
                break;
            }

            for (int i = 0; i < 5; i++) {
                if(n == i) { // 두 발이 같은 자리에 있을 수 없음
                    continue;
                }

                for(int j = 0; j < 5; j++) {
                    // 오른발을 옮겨 현재 모습이 됐을 때 최소 힘 저장하기
                    dp[s][i][n] = Math.min(dp[s - 1][i][j] + mp[j][n], dp[s][i][n]);
                }
            }

            for (int j = 0; j < 5; j++) {
                if(n == j) { // 두 발이 같은 자리에 있을 수 없음
                    continue;
                }

                for(int i = 0 ; i < 5; i++) {
                    // 왼발을 옮겨 현재 못브이 됐을 때 최소 힘 저장하기
                    dp[s][n][j] = Math.min(dp[s - 1][i][j] + mp[i][n], dp[s][n][j]);
                }
            }
            s++;
        }
        s--;

        int min = Integer.MAX_VALUE;
        for(int i = 0; i < 5; i++) {
            for(int j = 0; j < 5; j++) {
                min = Math.min(min, dp[s][i][j]); // 모두 수행했을 때 최솟값 찾기
            }
        }
        System.out.println(min); // 최솟값 출력하기
    }
}
```
