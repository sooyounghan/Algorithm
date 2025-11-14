-----
### 문제 40번 - 소수 구하기 (문제 1929번)
-----
1. 문제
```
M 이상 N 이하의 소수를 모두 출력하는 프로그램을 작성하시오.
```

2. 문제
   - 1번쨰 줄에 자연수 M과 N이 빈칸을 사이에 두고 주어짐 (1 ≤ M ≤ N ≤ 1,000,000)
   - M 이상 N 이하의 소수가 1개 이상 있는 입력만 주어짐

3. 출력 : 1줄에 1개씩, 증가하는 순서대로 소수 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/eee52e6c-13a3-441f-9ac2-2290da309b68">
</div>

4. 1단계 : 문제 분석하기
   - 숫자 사이 소수를 출력하는 문제
   - N의 최대 범위가 1,000,000이므로 일반적인 소수 구하기 방식으로 문제를 풀면 시간 초과 발생 : 따라서, 에라토스테네스의 체 방법으로 문제 해결
   - 일반적으로 소수를 찾을 때는 2 이상부터 자기 자신보다 작은 수로 나눴을 때 나머지가 0이 아닌 수를 찾음

5. 2단계
   - 크기가 N + 1인 배열을 선언한 후 값은 각각의 인덱스 값으로 채움
<div align="center">
<img src="https://github.com/user-attachments/assets/0d2da10f-9540-4be1-8b17-7935f80b07c1">
</div>

   - 1은 소수가 아니므로 삭제
<div align="center">
<img src="https://github.com/user-attachments/assets/5b238107-4149-4f3c-8202-c1b41e728f27">
</div>

   - 2부터 N의 제곱근까지 값을 탐색 : 값이 인덱스 값이면 그대로 두고, 그 값을 배수를 탐색해 0으로 변경
<div align="center">
<img src="https://github.com/user-attachments/assets/c61c51b3-e510-4d07-80bb-8c8d4b5b153d">
</div>

   - 배열에 남아 있는 수 중 M 이상 N 이하 수를 모두 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/606651ae-cc4e-425d-9c49-72fd0c6bfde5">
</div>

   - 💡 N의 제곱근까지만 탐색하는 이유
     + N이 a * b라고 가정했을 때, a와 b 둘이 동시에 N의 제곱근보다 클 수 없음
     + 따라서, N의 제곱근까지만 확인해도 전체 범위의 소수를 판별할 수 있음
     + 만약 16의 범위까지 소수를 구할 때, 16 = 4 * 4이므로 16보다 작은 숫자는 항상 4보다 작은 약수를 갖게됨
     + 따라서, 4까지만 확인하고 소수 판별 진행

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/9b1d0431-95cc-4ccb-8fbc-1a0f16e3742a">
</div>

7. 코드
```java
package numbertheory;

import java.util.Scanner;

public class PrimeNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int M = sc.nextInt();
        int N = sc.nextInt();
        int[] A = new int[N + 1];

        for (int i = 2; i <= N; i++) {
            A[i] = i;
        }

        for (int i = 2; i <= Math.sqrt(N); i++) { // 제곱근까지만 수행
            if(A[i] == 0) {
                continue;
            }

            for (int j = i + i; j <= N; j = j + i) { // 배수 지우기
                A[j] = 0;
            }
        }

        for (int i = M; i <= N; i++) {
            if(A[i] != 0) {
                System.out.println(A[i]);
            }
        }
    }
}
```
