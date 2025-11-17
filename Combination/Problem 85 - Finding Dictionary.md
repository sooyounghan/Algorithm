-----
### 문제 85 - 사전 찾기 (문제 1256번)
-----
1. 문제
```
동호와 규완이는 212호에서 문자열에 관해 공부하고 있다. 김진영 조교는 동호와 규완이에게 특별 과제를 줬다.
특별 과제는 특별한 문제로 이뤄진 사전을 만드는 것이다.
사전에 수록되어있는 모든 문자열은 N개의 "a"와 M개의 "z"로 이뤄져있다. 다른 문자는 없다. 사전에는 알파벳 순서대로 수록되어 있다.

규완이는 사전을 완성했지만, 동호는 사전을 완성하지 못했다.
동호는 자신의 과제를 끝내기 위해 사전을 몰래 참조하기로 했다.
동호는 규완이가 자리를 비운 사이에 몰래 사전을 보려고 하므로 문자열 1개만 찾을 여유밖에 없다.

N과 M이 주어졌을 때, 규완이의 사전에서 K번째 문자열이 무엇인지 구하는 프로그램을 구하시오.
```

2. 입력 : 1번쨰 줄에 N, M, K가 순서대로 주어짐 (N과 M은 100보다 작거나 같은 자연수, K는 1,000,000,000보다 작거나 같은 자연수)
3. 출력 : 1번쨰 줄에 규완이의 사전에서 K번째 문자열을 출력 (만약, 규완이의 사전에 수록되어 있는 문자열의 개수가 K보다 작으면 -1을 출력)
<div align="center">
<img src="https://github.com/user-attachments/assets/831b5747-6796-493e-941d-7234269c8471">
</div>

4. 1단계 : 문제 분석하기
   - 사전에 다루는 문자열이 a와 z밖에 없음
   - 핵심 아이디어는 a와 z의 개수가 각각 N, M개일 때, 이 문자들로 만들 수 있는 경우의 수는 N + M 개 중에서 N개를 뽑는 경우의 수 또는 M개를 뽑는 경우의 수와 동일

5. 2단계
   - 조합의 경우의 수를 나타내는 D 배열을 초기화하고, 점화식으로 값을 계산해 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/07f2ac28-0143-4d1c-ba22-344e47611a88">
</div>

   - 몇 번쨰 문자열을 표현해야 하는지를 나타내는 변수를 K라고 설정
     + 현재 자릿수에서 a를 선택했을 때, 남아 있는 문자들로 만들 수 있는 모든 경우의 수는 T
     + T와 K를 비교해 문자를 선택
<div align="center">
<img src="https://github.com/user-attachments/assets/7694a167-0837-4d06-b55c-797bfe01e001">
</div>

   - 두 번쨰 과정을 a와 z 문자 수를 합친 만큼 반복해 정답 문자열을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/e2863de5-c282-42ae-a2b2-f7cd0cdd161f">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/dee2c64c-8a35-42ed-afb9-c232bad223a5">
</div>

7. 코드
```java
package combination;

import java.util.Scanner;

public class Dictionary {
    public static int N, M, K;
    public static int[][] D;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        N = sc.nextInt();
        M = sc.nextInt();
        K = sc.nextInt();
        D = new int[202][202];

        // 조합 테이블 초기화
        for(int i = 0; i <= 200; i++) {
            for(int j = 0; j <= i; j++) {
                if (j == 0 || j == i) {
                    D[i][j] = 1;
                } else {
                    D[i][j] = D[i - 1][j - 1] + D[i - 1][j];
                    if (D[i][j] > 1000000000) {
                        D[i][j] = 1000000000; // K 범위를 넘어가면, 범위의 최댓값 저장
                    }
                }
            }
        }

        if(D[N + M][M] < K) { // 주어진 자릿수로 만들 수 없는 K번째 수이면
            System.out.println("-1");
        } else {
            while(!(N == 0 & M == 0)) {
                // a를 선택했을 때 남은 문자로 만들 수 있는 모든 경우의 수가 K보다 크다면,
                if(D[N - 1 + M][M] >= K) {
                    System.out.print("a");
                    N--;
                } else { // 모든 경우의 수가 K보다 작으면
                    System.out.print("z");
                    K = K - D[N - 1 + M][M]; // K값 업데이트
                    M--;
                }
            }
        }
    }
}
```
