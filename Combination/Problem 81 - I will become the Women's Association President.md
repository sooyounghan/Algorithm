-----
### 문제 81 - 부녀회장이 될 테야 (문제 2775번)
-----
1. 문제
```
평소 반상회에 참석하는 것을 좋아하는 주희는 이번 기회에 부녀회장이 되고 싶어 각 층의 사람들을 불러모아 반상회를 주최하려고 한다.
이 아파트에 거주를 하려면 조건이 있는데, 'a층의 b호에 살려면 자신의 아래층(a - 1)의 1호부터 b호까지 사람들 수의 합만큼 사람들을 데려와 살아야 한다.'라는 계약 조항을 꼭 지켜야 한다.

아파트에 비어 있는 집은 없고, 모든 거주민들이 이 계약 조건을 지켜왔다고 가정했을 때 주어지는 양의 정수 k와 n에 관해 k층 n호에는 몇 명이 살고 있는지 출력하라.
단, 아파트에는 0층부터 있고, 각 층에는 1호부터 있으며, 0층의 i호에는 i명이 산다.
```

2. 입력
   - 1번쨰 줄에 테스트 케이스의 수 T가 주어짐
   - 그리고 각각의 케이스마다 입력으로 1번째 줄에 정수 k, 2번쨰 줄에 정수 n이 주어짐

3. 출력 : 각각의 테스트 케이스에 해당 집의 거주민 수를 출력 (1 ≤ k, n ≤ 14)
<div align="center">
<img src="https://github.com/user-attachments/assets/8304a66f-1399-45c2-8549-e9e1be597271">
</div>

4. 1단계 : 문제 분석하기
   - 이 문제는 조합의 점화식을 도출하는 방법을 이용해 응용해 이 문제에서 사용할 점화식을 도출하면 쉽게 해결 가능
   - a층의 b호에 살려면 자신의 아래층(a - 1)의 1호부터 b호까지 사람들의 수의 합만큼 사람들을 데려와 살아야 한다는 내용은 다음과 같이 표현 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/81d361e2-62ab-4757-92c4-080ed8659986">
</div>

   - 위 내용을 좀 더 응용하면 다음과 같은 점화식으로 변경 가능 : a층 b호는 a층 b - 1호의 값에서 자기 아래층(a - 1층 b호)의 사람 수만 더하면 됨
     + 일반화된 점화식 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/b1fc3d21-77a4-458c-bb14-9e8c35f390e8">
</div>

   - 층의 수가 매우 적은 편이므로 가장 먼저 모든 아파트 층수에 관해 구해놓고, 테스트 케이스를 실행하는 방향의 구조

5. 2단계
   - DP 배열을 다음과 같이 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/bc67d0bf-6cee-46a2-b8b8-2eafac24df84">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/69f3d22f-f43a-4417-bdf1-d4e9e1ff2d6b">
</div>

   - DP 배열을 다음 점화식을 활용해 채움
<div align="center">
<img src="https://github.com/user-attachments/assets/78ae99ab-8e6e-408e-936b-fdb2d56a33cc">
</div>

   - 질의와 관련된 D[K][N] 값 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/9e8a62d7-bf67-4ed1-a17e-b3ad2d3fb850">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/d3440a70-d5b7-482d-b3f6-778f9ed1e7fa">
</div>

7. 코드
```java
package combination;

import java.util.Scanner;

public class WomenAssociationPresident {
    public static int T, N, K;
    public static int[][] D;

    public static void main(String[] args) {
        D = new int[15][15];

        for (int i = 0; i < 15; i++) { // 초기화
            D[i][1] = 1;
            D[0][i] = i;
        }

        for (int i = 1; i < 15; i++) {
            for (int j = 2; j < 15; j++) {
                D[i][j] = D[i][j - 1]  + D[i - 1][j]; // 점화식
            }
        }

        Scanner sc = new Scanner(System.in);

        T = sc.nextInt();
        for (int i = 0; i < T; i++) { // D 배열을 모두 구해 놓은 후 질의 출력
            K = sc.nextInt();
            N = sc.nextInt();
            System.out.println(D[K][N]);
        }
    }
}
```
