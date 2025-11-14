-----
### 문제 41 - 거의 소수 구하기 (문제 1456번)
-----
1. 문제
```
어떤 수가 소수의 N제곱(N ≥ 2)일 때, 이 수를 거의 '거의 소수'라고 한다.
A와 B가 주어질 때 A보다 크거나 같고, B보다 작거나 같은 거의 소수가 몇 개인지 출력하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 왼쪽 범위 A와 오른쪽 범위 B가 공백 한 칸을 두고 주어짐
   - A의 범위는 $10^{14}$보다 작거나 같은 자연수, B는 A보다 크거나 같고 $10^{14}$보다 작거나 같은 자연수

3. 출력 : 1번째 줄에 거의 소수가 총 몇 개 있는지 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/c4e58888-e645-4d29-b52e-67d80d122cdc">
</div>

4. 1단계 : 문제 분석하기
   - 최대 범위에 해당하는 모든 소수를 구해 놓고, 이 소수들을 N제곱한 값이 입력된 A와 B 사이에 존재하는지 판단해 문제 해결 가능
   - N ≥ 2이므로 입력에서 주어진 범위의 최댓값 $10^{14}$의 제곱근인 $10^{7}$까지 소수를 탐색해야 함
   - 에라토스테네스의 체를 이용해 빠르게 소수를 먼저 구한 뒤, 이후에는 주어진 소수들을 N 제곱한 값이 A ~ B 범위 안에 존재하는지 판별해 유효한 소수의 개수를 세면 이 문제를 해결 가능

5. 2단계
   - 2 ~ 10,000,000 사이에 존재하는 모든 소수 구하기 (31보다 큰 수에서는 거듭제곱 한 값이 1,000을 넘어가므로 그림에는 표시하지 않음)
<div align="center">
<img src="https://github.com/user-attachments/assets/40487601-e89e-4a24-b848-8a7cba1a5024">
</div>

   - 각각의 소수에 관해 N제곱한 값이 B보다 커질 때까지 반복문을 실행
     + 이 때 소수를 N제곱한 값이 A보다 크거나 같으면 거의 소수로 판단해 카운트
     + 모든 소수에 관해서 반복문을 실행한 후 카운트한 값을 출력
     + N제곱한 값이 long형 변수의 범위를 넘을 수 있어 이항 정리로 해결
<div align="center">
<img src="https://github.com/user-attachments/assets/fcb3a889-8fdd-47ca-9137-73611f546aeb">
</div>

   - 이 부분을 실제 구현하면 N제곱한 값을 구하는 도중 값의 범위가 long형을 초과하는 경우 발생
   - 따라서, 계산 오류를 방지하려면 $N^{k}$과 B 값이 아닌 N과 B / $N^{k-1}$을 비교하는 형식으로 식을 적절하게 정리해야 함

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/02e3c59c-ab19-456a-8add-05c4eae9e8cf">
</div>

7. 코드
```java
package numbertheory;

import java.util.Scanner;

public class AlmostPrimeNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        long Min = sc.nextLong();
        long Max = sc.nextLong();
        long[] A = new long[10000001];

        for (int i = 2; i < A.length; i++) {
            A[i] = i;
        }

        for (int i = 2; i <= Math.sqrt(A.length); i++) { // 제곱근까지만 수행
            if (A[i] == 0) {
                continue;
            }

            for (int j = i + i; j < A.length; j = j + i) { // 배수 지우기
                A[j] = 0;
            }
        }

        int count = 0;
        for (int i = 2; i < 10000001; i++) {
            if(A[i] != 0) {
                long temp = A[i];

                while((double)A[i] <= (double)Max / (double)temp) {
                    if((double)A[i] >= (double)Min / (double)temp) {
                        count++;
                    }

                    temp = temp * A[i];
                }
            }
        }
        System.out.println(count);
    }
}
```
