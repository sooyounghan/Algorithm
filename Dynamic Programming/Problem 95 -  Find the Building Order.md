-----
### 문제 95 - 빌딩 순서 구하기
-----
1. 문제
```
상근이가 살고 있는 동네에는 빌딩 N개가 1줄로 세워져 있다.

모든 빌딩의 높이는 1보다 크거나 같고, N보다 작거나 같으며, 높이가 같은 빌딩은 없다.
상근이는 학교 가는 길에 가장 왼쪽에 서서 빌딩을 몇 개 볼 수 있는지 봤고, 집에 돌아오는 길에는 가장 오른쪽에 서서 빌딩을 몇 개 볼 수 있는지 봤다.

상근이는 가장 왼쪽과 오른쪽에서만 빌딩을 봤기 때문에 빌딩이 어떤 순서로 위치해있는지 알 수가 없다.
빌딩의 개수 N과 가장 왼쪽에서 봤을 때 보이는 빌딩의 수 L, 가장 오른쪽에서 봤을 때 보이는 빌딩의 수 R이 주어졌을 때, 가능한 빌딩 순서 경우의 수를 구하는 프로그램을 작성하시오.

예를 들어, N = 5, L = 3, R = 2일 때, 가능한 빌딩 배치 중 하나는 1 3 5 2 4이다.
```

2. 입력 : 1번째 줄에 빌딩의 개수 N과 가장 왼쪽에서 봤을 때 보이는 빌딩의 수 L, 가장 오른쪽에서 봤을 때 보이는 빌딩의 수 R이 주어짐 (1 ≤ N ≤ 100, 1 ≤ L, R ≤ N)
3. 출력 : 1번쨰 줄에 가능한 빌딩 순서의 경우의 수를 1,000,000,007으로 나눈 나머지를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/0898a054-52dd-4257-8584-eebf8aa7f638">
</div>

4. 1단계 : 문제 분석하기
   - 점화식의 의미
<div align="center">
<img src="https://github.com/user-attachments/assets/2bd51b06-d46a-4a1f-8de8-af2dfe54e971">
</div>

   - 점화식을 적절하게 정의하고 난 뒤에는, 이 문제를 어떻게 하면 단순화할 지 생각해야 함
     + 먼저 N - 1개의 빌딩과 관련된 모든 경우의 수를 알고 있다고 가정 : 그러면 이후, 1개의 빌딩을 어느 곳에 배치할 것인지 결정하는 것이 관건
       * 이 떄, 배치하는 빌딩이 가장 크다면, 가장 왼쪽이나 오른쪽에 배치할 때, 보이는 빌딩의 수는 1개가 될 것
       * 하지만, 중간에 배치하면 어떤 수가 나올지 예상하기 어려움
     + 만약, '높이가 애매한 빌딩을 마지막에 배치하는 것이 아니라, 일정한 규칙에 따라 배치해 단순화'할 수 있다면?
       * 가장 큰 빌딩을 마지막에 배치하면, 중간에 배치했을 때 경우가 복잡하므로, 이와 반대로 가장 작은 빌딩을 N번째로 배치한다고 가정
       * 다음과 같이 3가지 경우의 수 발생
<div align="center">
<img src="https://github.com/user-attachments/assets/0600bfad-639c-4694-a1d8-f8c40d40b002">
</div>

   - 3가지 경우의 수를 바탕으로 점화식을 도출

5. 2단계
   - 상황에 따른 점화식을 구하기
   - 첫 번째 : 먼저 N개의 빌딩이 왼쪽에 L개, 오른쪽에 R개가 보인다고 가정하면, N - 1개의 빌딩에서 왼쪽에 빌딩을 추가할 때, 왼쪽 빌딩이 1개 증가하므로 이전 경으이 수는 다음과 같음
<div align="center">
<img src="https://github.com/user-attachments/assets/bbb56c21-0d46-433d-8f65-66308132b3c5">
</div>

   - 두 번째 : N - 1개의 빌딩에서 오른쪽에 빌딩을 추가할 때, 오른쪽 빌딩이 1개 증가하므로 이전 경우의 수
<div align="center">
<img src="https://github.com/user-attachments/assets/5f4f98ad-244f-4b2b-a423-d6a1b2fe2d8a">
</div>

   - 세 번째 : N - 1개의 빌딩에서 가운데 빌딩을 추가할 때는 증가 수는 없지만, N - 2개 위치에 배치할 수 있으므로 N - 2를 곱함
<div align="center">
<img src="https://github.com/user-attachments/assets/4d119afa-d8da-440d-8277-18d3afbac63d">
</div>

   - 3가지 경우의 수를 모두 더하면 다음과 같은 점화식이 나옴
<div align="center">
<img src="https://github.com/user-attachments/assets/17a286a6-16f7-43a3-a8ad-f54e3526ba9e">
</div>

   - D 배열을 초기화 : 건물이 1개면, 경우의 수는 1개일 수 밖에 없음
<div align="center">
<img src="https://github.com/user-attachments/assets/6c137d41-216a-4c69-9a0e-a13c6809bfce">
</div>

   - 점화식을 이용해 구하기
<div align="center">
<img src="https://github.com/user-attachments/assets/db37fcd1-0261-4194-8035-64cbe982fa7d">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/32eda1c3-0e82-4714-9410-df0a40b13f3f">
</div>

7. 코드
```java
package dynamicprogramming;

import java.util.Scanner;

public class BuildingOrder {
    public static long mod = 1000000007;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int L = sc.nextInt();
        int R = sc.nextInt();

        long[][][] D = new long[101][101][101];

        D[1][1][1] = 1; // 빌딩이 1개이면 놓을 수 있는 경우의 수는 1개
        
        for(int i = 2; i <= N; i++) {
            for(int j = 1; j <= L; j++) {
                for(int k = 1; k <= R; k++) {
                    D[i][j][k] = (D[i - 1][j][k] * (i - 2) + (D[i - 1][j][k - 1] + D[i - 1][j - 1][k])) % mod;
                }
            }
        }

        System.out.println(D[N][L][R]);
    }
}
```
