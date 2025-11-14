-----
### 문제 27 - N-Queen 배치하기 (문제 9663번)
-----
1. 문제
```
N-Queen 문제에는 크기가 N X N인 체스판 위에 퀸 N개를 서로 공격할 수 없게 놓아야 한다.
N이 주어졌을 때, 퀸을 놓는 경우의 수를 구하는 프로그램을 작성하시오.
```

2. 입력 : 1번쨰 줄에 N(1 ≤ N < 15)이 주어짐
3. 출력 : 1번째 줄에 퀸 N개를 서로 공격할 수 없게 놓는 경우의 수 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/75005c2d-5648-479b-99d1-788460e6b43f">
</div>

4. 1단계 : 문제 분석하기
   - N의 크기가 최대 15이므로 시간 복잡도는 크게 걱정할 필요가 없음
   - 체스에서 퀸은 가로 / 세로 / 대각선으로 공격할 수 있으므로 같은 행에는 퀸이 1개만 존재할 수 있음
     + 💡 그리고 이를 확장해서 생각해보면, 결국 모든 행에 퀸을 1개씩 배치해야 N개의 퀸을 서로 공격하지 않도록 놓을 수 있음
   - 따라서 맨 위행부터 시작해 각 행마다 퀸을 하나씩 배치하며 가능한 모든 경우의 수를 찾는 방식으로 접근해야 함
   - 이 때, 백트래킹 알고리즘을 사용하여 구하면 됨
     + 가능한 선택지 탐색 : 현재 행에서 퀸을 놓을 수 있는 위치 탐색
     + 유효성 검사 및 가지치기 : 해당 위치에 퀸을 배치할 때 다른 퀸과 서로 공격할 수 있는지 확인하며, 공격할 수 있다면 탐색을 종료하고 이전 단계로 돌아감
     + 해답 도출 : 마지막 행까지 퀸 배치를 모두 완료하면 경우의 수를 1 증가시키고, 백트래킹 전체를 종료하면 경우의 수 출력

5. 2단계
   - N이 4라고 가정하고 문제 해결
   - 1번째 행부터 퀸을 배치할 수 있는 모든 위치 탐색
<div align="center">
<img src="https://github.com/user-attachments/assets/b04a2a85-ba0e-43fa-99e9-e46b039307e1">
</div>

   - 기본적으로 후보가 될 수 있는 위치는 해당 행의 모든 열
   - 퀸을 배치한 후에는 이전 행들에 배치되어 있는 퀸들과 서로 공격할 수 있는지 검사
   - 퀸은 직선과 대각선 공격을 할 수 있으므로, 공격 가능 여부는 같은 열이나 대각선 상 퀸이 2개 이상 존재하는지 여부로 검사
   - 만약 공격할 수 있다면 해당 배치는 가지치기를 수행하여 탐색 종료

   - 이렇게 백트래킹 알고리즘을 수행하면서 마지막 행까지 퀸 배치를 완료하면 유효한 경우로 판단하여 경우의 수를 1 증가
   - 알고리즘은 모두 수행 완료한 후 경우의 수를 출력

   - 추가로 N X N인 체스판이 주어지므로 문제를 이차원 배열로 생각할 수 있지만, 일차원 배열로도 퀸 배치 정보를 충분히 저장 가능 (일차원 배열의 인덱스를 행, 값을 열이라고 생각하는 것)
<div align="center">
<img src="https://github.com/user-attachments/assets/1c3f1b3e-3158-4d64-bd85-7c19a1f953da">
</div>

   - 퀸의 배치 정보를 일차원 배열로 저장하면 공격 가능 여부를 쉽게 구현 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/e75a4b32-25b9-4e97-8c67-e1a98ab5de4e">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/a2473f0a-ee15-4e05-8463-6f90b2e8bc7f">
</div>

   - 퀸의 배치 정보를 일차원 배열로 표현하여 구현

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/4b0a7157-bf27-4ac9-975b-77ccb10873e4">
</div>

7. 코드
```java
package search.backtracking;

import java.util.Scanner;

public class NQueen {
    public static int[] A; // 퀸 배치 정보 저장
    public static int N; // 체스판 크기 N X N
    public static int cnt = 0; // 퀸을 배치하는 경우의 수 저장

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        N = sc.nextInt();
        A = new int[N];
        backtracking(0);

        System.out.println(cnt);
    }

    private static void backtracking(int row) {
        if(row == N) { // 퀸 N개를 모두 배치한 경우
            cnt++;;
            return;
        }

        for(int i = 0; i < N; i++) {
            A[row] = i;

            if (check(row)) {  // 배치한 퀸이 이전 퀸들과 서로 공격할 수 없는지 체크
                backtracking(row + 1);
            }
        }
    }

    private static boolean check(int row) {
        for (int i = 0; i < row; i++) {
            if(A[i] == A[row]) { // 일직선 배치
                return false;
            }

            if(Math.abs(row - i) == Math.abs(A[i] - A[row])) { // 대각선 배치
                return false;
            }
        }

        return true;
    }
}
```
