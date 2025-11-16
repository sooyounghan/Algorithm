-----
### 문제 82 - 다리 놓기 (문제 1010번)
-----
1. 문제
```
강 주변에서 다리를 짓기에 적합한 곳을 사이트라고 한다.
강의 서쪽에는 N개, 동쪽에는 M개의 사이트가 있다. (N ≤ M)
서쪽의 사이트와 동쪽의 사이트를 다리로 연결하려고 한다.

이 때 한 사이트에는 최대 1개의 다리만 연결할 수 있다.

다리를 최대한 많이 지으려고 하므로 서쪽의 사이트 개수만큼 N만큼 다리를 지으려고 한다.
다리끼리는 서로 겹쳐질 수 없다고 할 때, 다리를 지을 수 있는 경우의 수를 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 테스트 케이스의 개수 T
   - 그다음 줄부터 각 테스트 케이스에서 사이트의 개수가 정수 N, M (0 < N ≤ M < 30)으로 주어짐

3. 출력 : 각 테스트 케이스에서 다리를 지을 수 있는 경우의 수를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/86a36eae-7c45-449c-a937-a7c68bea26d1">
</div>

4. 1단계 : 문제 분석하기
   - 문제의 핵심 : 문제의 내용을 읽고 조합 문제로 생각할 수 있는가?
   - 특히 다리끼리 서로 겹쳐질 수 없다는 조건이 이 문제를 쉽게 구성 : 이 조건으로 인해 이 문제를 M개의 사이트에서 N개를 선택하는 문제로 변경할 수 있음
     + 겹치지 않게 하려면 동쪽에서 N개를 선택한 후, 서쪽과 동쪽의 가장 위쪽 사이트에서부터 차례대로 연결할 수 밖에 없음
   - 결국 이 문제는 M개에서 N개를 뽑는 경우의 수를 구하는 조합 문제로 변형해 풀 수 있음

5. 2단계
   - D[31][31]로 DP 배열을 선언하고, 값을 다음 조건에 따라 초기화 : 기존의 조합 문제에서 초기화할 때와 같은 조건
<div align="center">
<img src="https://github.com/user-attachments/assets/f0f64c8a-694e-42a1-af3d-3e17b4f61cec">
</div>

   - 점화식을 이용해 DP 배열을 채움 : 이 때, N과 M의 최댓값이 30보다 작으므로 미리 D 배열의 값을 30까지 구함
<div align="center">
<img src="https://github.com/user-attachments/assets/43086ea4-f930-44e5-b3ac-2c2e19563d40">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/4c2a50e2-8172-4482-8c9a-4350faed70b1">
</div>

   - 테스트 케이스를 실행해 D[M][N]의 값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/cda00f27-4808-450b-bff9-c42713af1e60">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/2c5129fa-83b2-4fb1-94ed-f257aeb10edd">
</div>

7. 코드
```java
package combination;

import java.util.Scanner;

public class BuildingBridge {
    public static long[][] D;

    public static void main(String[] args) {
        D = new long[31][31];

        for(int i = 0; i <= 30; i++) {
            D[i][1] = i;
            D[i][0] = 1;
            D[i][i] = 1;
        }

        for(int i = 2; i <= 30; i++) {
            for (int j = 1; j < i; j++) {
                D[i][j] = D[i - 1][j] + D[i - 1][j - 1];
            }
        }

        Scanner sc = new Scanner(System.in);

        int t = sc.nextInt();

        for(int i = 0; i < t; i++) {
            int N = sc.nextInt();
            int M = sc.nextInt();
            System.out.println(D[M][N]);
        }
    }
}
```
