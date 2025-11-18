-----
### 문제 97 - 행렬 곱 연산 횟수의  최솟값 구하기 (문제 11049번)
-----
1. 문제
```
크기가 N X M인 행렬 A와 M X K인 B를 곱할 때, 필요한 곱셈 연산 횟수는 N X M X K번이다.
행렬 N개를 곱하는 데 필요한 곱셈 연산 횟수는 행렬을 곱하는 순서에 따라 달라진다.

예를 들어, A의 크기가 5 X 3이고, B의 크기가 3 X 2이고, C의 크기가 2 X 6일 때, 행렬의 곱 ABC를 구하는 경우를 생각해보자.
AB를 먼저 곱하고, C를 곱하는 경우 (AB)C에 필요한 곱셈 연산 횟수는 (5 X 3 X 2) + (5 X 2 X 6) = 30 + 60 = 90번이다.
BC를 먼저 곱한 후, A를 곱하는 경우 A(BC)에 필요한 곱셈 연산 횟수는 (3 X 2 X 6) + (5 X 3 X 6) = 36 + 90 = 126번이다.

같은 곱셈이지만, 곱셈을 하는 순서에 다라 곱셈 연산 횟수가 달라진다.

행렬 N개의 크기가 주어졌을 때, 모든 행렬을 곱하는 데 필요한 곱셈 연산 횟수의 최솟값을 구하는 프로그램을 작성하시오.
단, 일벽으로 주어진 행렬의 순서를 바꾸면 안 된다.
```

2. 입력
   - 1번째 줄에 행렬의 개수 N (1 ≤ N ≤ 500)이 주어짐
   - 2번쨰 줄부터 N개의 줄에 행렬의 크기 r과 c가 주어짐 (1 ≤ r, c ≤ 500)
   - 항상 순서대로 곱셈할 수 있는 크기만 입력으로 주어짐

3. 출력
   - 1번쨰 줄에 입력으로 주어진 행렬을 곱하는 데 필요한 곱셈 연산 횟수의 최솟값을 출력
   - 정답은 $2^{31} - 1$보다 작거나 같은 자연수이며, 또한, 최악의 순서로 연산해도 연산 횟수가 $2^{31} - 1$보다 작거나 같음
<div align="center">
<img src="https://github.com/user-attachments/assets/34a37c28-7cd7-4c46-9517-c114f13eac20">
</div>

4. 1단계 : 문제 분석하기
   - 부분 문제를 구해 큰 문제를 해결하는 방식이 동적 계획법의 특징 중 하나이므로, 부분 문제가 해결됐다고 가정하고, 점화식을 떠올려 볼 것
   - 행렬의 개수 N이 주어지고, 1 ~ N개를 모두 곱했을 때 최소 연산 횟수를 구해야 함
     + 만약, N개 이외의 부분 영역, 예를 들면 1 ~ N - 1 / 2 ~ N / 3 ~ N - 2 등 N을 제외한 모든 부분 구역을 1개의 행렬로 만드는 데 필요한 최소 연산 횟수를 알고 있다고 가정
     + 이러한 가정을 바탕으로 1 ~ N 구역의 최소 연산 횟수에 대한 점화식 정의
<div align="center">
<img src="https://github.com/user-attachments/assets/42e92034-d8cd-48d0-836c-57a73c948329">
</div>  

   - N번째 행렬을 제외한 모든 행렬이 합쳐진 경우
<div align="center">
<img src="https://github.com/user-attachments/assets/b578d523-9b73-4113-b418-a42c090ad144">
</div>

   - D[1][N - 1], D[N][N]의 값을 안다고 가정했으므로, 이 때, 1개의 행렬을 합치는 데 드는 횟수
<div align="center">
<img src="https://github.com/user-attachments/assets/55150c7b-55d2-40a8-8a96-702e52273f62">
</div>

   - a는 두 행렬을 합치는 데, 드는 값으로 2 X 3과 3 X 6 행렬이 있다면, 2 X 3 X 6 = 36 : 이 떄는, 1번째 행렬의 row, 개수, N번쨰 행렬의 row 개수, column 개수를 곱하면 됨
<div align="center">
<img src="https://github.com/user-attachments/assets/1b8ba599-2562-4754-b359-691a2dd3bd12">
</div>

   - D[1][N]의 값을 도출 가능

5. 2단계
   - 행렬 구간에 행렬이 1개일 때 0을 반환
   - 행렬 구간에 행렬이 2개일 때, '앞 행렬의 row 값 * 뒤 행렬의 row 값 * 뒤 행렬의 column 값'을 반환
   - 행렬 구간에 행렬이 3개 이상일 때, 조건식
<div align="center">
<img src="https://github.com/user-attachments/assets/b5d3055f-f7eb-4564-b35e-8019cf1a9742">
</div>

   - 만약 구하려는 영역의 행렬 개수가 3개 이상일 때는 이 영역을 다시 재귀 형식으로 쪼개면서 계산하면 됨
   - 점화식을 재귀 함수 형태, 즉 톱-다운 방식으로 구현

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/8bff3581-54a2-4088-ab9d-e2de32798989">
</div>

7. 코드
```java
package dynamicprogramming;

import java.util.Scanner;

public class MatrixMultipleCount {
    public static int N;
    public static Matrix[] M;
    public static int[][] D;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        N = sc.nextInt();
        M = new Matrix[N + 1];
        D = new int[N + 1][N + 1];

        for (int i = 0; i < D.length; i++) {
            for (int j = 0; j < D[i].length; j++) {
                D[i][j] = -1;
            }
        }

        for (int i = 1; i <= N; i++) {
            int y = sc.nextInt();
            int x = sc.nextInt();

            M[i] = new Matrix(y, x);
        }

        System.out.println(execute(1, N));
    }

    // 톱-다운 방식으로 점화식 함수 구현
    private static int execute(int s, int e) {
        int result = Integer.MAX_VALUE;

        if(D[s][e] != -1){ // 계산했던 부분이면 다시 계산하지 않음 (메모이제이션)
            return D[s][e];
        }

        if(s == e) { // 행렬 1개의 곱셈 연산의 수
            return 0;
        }

        if(s + 1 == e) { // 행렬 2개의 곱셈 연산의 수
            return M[s].y * M[s].x * M[e].x;
        }

        for(int i = s; i < e; i++) { // 행렬이 3개 이상일 때 곱셈 연산의 수 : 점화식 처리
            result = Math.min(result, M[s].y * M[i].x * M[e].x + execute(s, i) + execute(i + 1, e));
        }
            return D[s][e] = result;
    }

    // 행렬 정보 저장 클래스
    static class Matrix {
        private int y;
        private int x;

        public Matrix(int y, int x) {
            this.y = y;
            this.x = x;
        }
    }
}
```
