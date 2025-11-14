-----
### 문제 43 - 제곱이 아닌 수 찾기 (문제 1016번)
-----
1. 문제
```
어떤 수 X가 1보다 큰 제곱수로 나누어떨어지지 않을 때, 이 수를 '제곱이 아닌 수'라고 가정해보자.
여기서 제곱수는 정수의 제곱이다.
min과 max의 값이 주어질 때, min보다 크고, max보다 작은 값 중 '제곱이 아닌 수'가 몇 개 있는지 출력하시오.
```

2. 입력 : 1번째 줄에 두 정수 min과 max가 주어짐
3. 출력 : 1번쨰 줄에 [min, max] 구간에 제곱이 아닌 수가 몇 개인지 출력 (1 ≤ min ≤ 1,000,000,000,000, min ≤ max ≤ min + 1,000,000)
<div align="center">
<img src="https://github.com/user-attachments/assets/296e2ffc-e1e0-48b2-b570-42baa50dcd24">
</div>

4. 1단계 : 문제 분석하기
   - min의 최댓값이 1,000,000,000,000으로 매우 큰 것 같지만 실제로는 min과 max 사이의 수들 안에서 구하는 것이므로 1,000,000개의 데이터만 확인하면 됨
   - 제곱수 판별을 일반적인 반복문으로 구하면 시간 초과가 발생하므로 에라토스테네스의 체 알고리즘 방식을 제곱수 판별 로직에 적용해 문제 해결

5. 2단계
   - 2의 제곱수인 4부터 max인 10까지의 제곱수 찾기
<div align="center">
<img src="https://github.com/user-attachments/assets/7b79a695-fc40-47b3-8f6c-00c7e26d11c7">
</div>

   - 탐색한 배열에서 제곱수로 확인되지 않은 수의 개수를 센 후 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/88cc6e6e-c1e1-498a-bd50-f1dbd7c4b9ea">
</div>

   - 데이터를 순차적으로 탐색하는 것이 아니라 에라토스테네스 체 방식으로 제곱수의 배수 형태로 탐색해 시간 복잡도를 최소화하는 것이 핵심

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/9a0fa72b-0a45-4908-a93a-8f15d78c0e19">
</div>

7. 코드
```java
package numbertheory;

import java.util.Scanner;

public class NotSquareNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        long Min = sc.nextLong();
        long Max = sc.nextLong();

        // 최댓값과 최솟갑의 차이만큼 배열 선언
        boolean[] Check = new boolean[(int) (Max - Min + 1)];

        // 2의 제곱수인 4부터 Max보다 작거나 같은 값까지 반복
        for(long i = 2; i * i <= Max; i++) {
            long pow = i * i; // 제곱수
            long start_index = Min / pow;

            if(Min % pow != 0) {
                start_index++; // 나머지가 있으면 1을 더해야 Min보다 큰 제곱수에서 시작
            }

            for(long j = start_index; pow * j <= Max; j++) { // 제곱수
                Check[(int) ((j * pow) - Min)] = true;
            }
        }

        int count = 0;
        for (int i = 0; i <= Max - Min; i++) {
            if(!Check[i]) {
                count++;
            }
        }

        System.out.println(count);
    }
}
```
