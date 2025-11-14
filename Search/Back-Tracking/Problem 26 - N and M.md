-----
### 문제 26 - N과 M (문제 15649번)
-----
1. 문제
```
자연수 N과 M이 주어졌을 때, 다음 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

[조건 : 1부터 N까지 자연수 중 중복 없이 M개를 고른 수열]
```

2. 입력 : 1번째 줄에 자연수 N과 M(1 ≤ M ≤ N ≤ 8)이 주어짐
3. 출력
   - 한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력
   - 각 수열은 숫자를 공백으로 구분해서 출력해야 하며, 중복되는 수열을 여러 번 출력하면 안 됨
   - 수열은 사전순으로 오름차순 정렬하여  출력해야 함
<div align="center">
<img src="https://github.com/user-attachments/assets/bdb4c69c-d16f-4298-b864-9b22379082fd">
</div>

4. 1단계 : 문제 분석하기
   - N과 M의 범위가 8 이하이므로 시간 복잡도에서 비교적 자유로움
   - 길이가 M인 모든 수열의 경우의 수를 구하는 것이 목표이므로, 백트래킹 알고리즘을 이용해 가능한 모든 경우를 구하면 문제의 답 도출 가능
     + 가능한 선택지 탐색 : 현재 수열에서 추가할 수 있는 자연수를 탐색
     + 유효성 검사 및 가지치기 : 자연수가 기존 수열에서 이미 사용한 수라면, 해당 수를 선택한 탐색은 진행하지 않고 이전 단계로 돌아감
     + 해답 도출 : 수열의 길이가 M이 될 때 해당 수열의 정보를 출력

5. 2단계
   - 예제 2번을 예시로 시작 : 먼저 가능한 선택지를 탐색
     + N 이하의 자연수를 수열의 수로 선택할 수 있으므로, 가능한 선택지는 1, 2, 3, 4
     + 여기서 한가지 팁 : 선택지를 작은 수부터 먼저 탐색하도록 기준을 잡으면, 문제에서 요구한 사전순 출력 조건을 자연스럽게 만족할 수 있음

   - 1, 2, 3, 4 선택지에서 더 이상 탐색이 불필요한 경로는 가지치기로 탐색을 중단
     + 이 때 가지치기의 기준은 이미 수열에 포함한 수
     + 같은 수를 여러 번 사용할 수 없음

   - 마지막으로 수열의 길이가 문제에서 요구한 M이 되면 해당 수열 정보 출력
   - 만족하는 조건에 도달하여 답을 출력하거나 가지치기로 탐색을 멈춘 뒤에는 다른 경우의 수를 탐색해야 하므로 바로 이전 단계로 돌아감
     + 이전 단계로 돌아가는 로직은 재귀 함수 형태로 자연스럽게 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/cba89385-1b57-4e57-82ad-b1ac62ddfb71">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/2ed52168-0acf-433d-b423-f9d4405b7f7a">
</div>

7. 코드
```java
package search.backtracking;

import java.util.Scanner;

public class NandM {
    public static int N, M;
    public static boolean[] V; // 숫자 사용 여부 저장
    public static int[] S; // 수열 정보 저장

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        N = sc.nextInt();
        M = sc.nextInt();
        S = new int[N];
        V = new boolean[N];
        backtracking(0);
    }

    private static void backtracking(int length) {
        if (length == M) { // 길이가 M인 수열이 만들어진 경우
            printArray();
            return;
        }

        for (int i = 0; i < N; i++) {
            if (!V[i]) {
                V[i] = true; // 수 사용 저장하기
                S[length] = i; // 수열에 수 저장하기
                backtracking(length + 1);
                V[i] = false; // 수 반납 저장하기
            }
        }
    }

    private static void printArray() {
        for(int i = 0; i < M; i++) {
            System.out.print(S[i] + 1 + " "); // i는 0부터 시작하므로 +1
        }
        System.out.println();
    }
}
```
