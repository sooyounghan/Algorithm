-----
### 문제 88 - 퇴사 준비하기 (문제 14501번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/bcc67fd4-6b5b-4df1-bf17-629227d20f5a">
</div>

2. 입력
   - 1번째 줄에 N(1 ≤ N ≤ 15)이 주어짐
   - 2번째 줄부터 N개의 줄에 $T_{i}$와 $P_{i}$가 공백으로 구분되어 주어지고, 1일에서 N일까지 순서대로 주어짐 1 ≤ ($T_{i}$ ≤ 5, 1 ≤ $P_{i}$ ≤ 1,000)

3. 출력 : 1번째 줄에 백준이가 얻을 수 있는 최대 이익을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/e84853d0-ac77-42e2-90f1-940da945e04c">
</div>

4. 1단계 : 문제 분석하기
   - 💡 동적 계획법에서 점화식은 유일하지 않음 : 다양한 자신만의 아이디어로 적절한 점화식을 찾는 훈련을 반복하는 것이 중요함
   - 먼저 점화식의 형태를 정의
     + 문제의 주요 요소가 날짜 1개 정도라고 판단되어 1차원 형태의 점화식(D[i])을 세움
     + 그 다음, D[i]의 의미를 정해야 함
     + D[i]의 의미 : i번째 날부터 퇴사일까지 벌 수 있는 최대 수입으로 정의해 문제에 접근

5. 2단계
   - 점화식의 형태와 의미 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/29cd744e-8b52-4c3f-bbfb-3dae278dab66">
</div>

   - 점화식 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/91a20382-cd2d-4d56-a0e5-a22ed8b90705">
</div>

   - 점화식을 이용해 D배열을 채움
<div align="center">
<img src="https://github.com/user-attachments/assets/c335e3dd-530e-418b-a625-9ea6353d1598">
</div>

   - 완성된 D 배열에서 D[1]값 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/3b551e9b-3208-4438-b5c7-a0d999661eec">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/6a010b56-bf86-492f-8f0d-5752786a1f29">
</div>

4. 코드
```java
package dynamicprogramming;

import java.util.Scanner;

public class ReadyLeaveWork {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int[] D = new int[N + 2]; // 오늘부터 퇴사일까지 벌 수 있는 최대 수입
        int[] T = new int[N + 1];
        int[] P = new int[N + 1];

        for(int i = 1; i <= N; i++) {
            T[i] = sc.nextInt();
            P[i] = sc.nextInt();
        }

        for(int i = N; i > 0; i--) {
            if(i + T[i] > N + 1) {
                D[i] = D[i + 1];
            } else {
                D[i] = Math.max(D[i + 1], P[i] + D[i + T[i]]);
            }
        }

        System.out.println(D[1]);
    }
}
```
