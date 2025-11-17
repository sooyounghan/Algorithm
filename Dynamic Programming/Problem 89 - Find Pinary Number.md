-----
### 문제 89 - 이친수 구하기 (문제 2193번)
-----
1. 문제
```
0과 1로만 이뤄진 수를 '이진수'라고 한다.
이러한 이진수 중 특별한 성질을 갖는 것들이 있는데, 이들을 '이친수(Pinary Number)'라고 한다.

다음의 성질을 만족한다.
   1. 이친수는 0으로 시작하지 않는다.
   2. 이친수에서는 1이 두 번 연속으로 나타나지 않는다. 즉, 11을 부분 문자열로 갖지 않는다.

예를 들면 1, 10, 100, 101, 1000, 1001등이 이친수가 된다.
하지만 0010101이나 101101은 각각 1, 2번 규칙에 위배되므로 이친수가 아니다
N(1 ≤ N ≤ 90)이 주어졌을 때, N자리 이친수의 개수를 구하는 프로그램을 작성하시오.
```

2. 입력 : 1번째 줄에 N이 주어짐
3. 출력 : 1번째 줄에 N자리 이친수의 개수를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/1481590e-e40f-4092-b69d-6b7dffd50109">
</div>

4. 1단계 : 문제 분석하기
   - 이친수의 개수와 관련된 요소를 먼저 찾아보면 가장 먼저 자릿수가 중요하다고 판단
   - 더 나아가 0으로 끝나는 이친수와 1로 끝나는 이친수를 구분해서 생각
   - 이 문제에서 점화식은 유일하지 않고, 다양할 수 있음
   - 여기서는 2차원 배열 점화식을 선언(D[N][2])하고, 접근

5. 2단계
   - 점화식의 형태와 의미 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/9524d15c-5d2f-4b6d-a9f6-fe7491bfe432">
</div>

   - 점화식 도출 : 1은 두 번 연속으로 나오지 않는다는 조건이 이 점화식을 구하는 핵심
<div align="center">
<img src="https://github.com/user-attachments/assets/e4b61add-edf0-4e22-bf65-86150f463251">
</div>

   - 점화식을 이용해 D 배열을 채운 후 D[N][0] + D[N][1]의 값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/8e1ac16b-0d15-4bc6-b60d-e0c2d02a4028">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/d83eb653-c46f-49a0-806d-b10dd41e84a4">
</div>

7. 코드
```java
package dynamicprogramming;

import java.util.Scanner;

public class PinaryNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        long[][] D = new long[N + 1][2];

        D[1][1] = 1; // 1자리 이친수 1 한가지
        D[1][0] = 0;

        for(int i = 2; i <= N; i++) {
            D[i][0] = D[i - 1][1] + D[i - 1][0];
            D[i][1] = D[i - 1][0];
        }

        System.out.println(D[N][0] + D[N][1]);
    }
}
```
