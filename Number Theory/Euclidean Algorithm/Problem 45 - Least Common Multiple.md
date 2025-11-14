-----
### 문제 45 - 최소 공배수 구학 (문제 1934번)
-----
1. 문제
```
두 자연수 A와 B가 있을 때, A의 배수이면서 B의 배수인 자연수를 A와 B의 공배수라고 한다.
이런 공배수 중 가장 작은 수를 최소 공배수라고 한다.
예를 들어 6과 15의 공배수는 30, 60, 90 등이 있으며, 최소 공배수는 30이다.

두 자연수 A와 B가 주어졌을 때, A와 B의 최소 공배수를 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 테스트 케이스 개수 T(1 ≤ T ≤ 1,000)
   - 2번째 줄부터 T개의 줄에 걸쳐 A와 B가 주어짐 (1 ≤ A, B ≤ 45,000)

3. 출력 : 1번쨰 줄부터 T개의 줄에 A와 B의 최소 공배수를 입력받은 순서대로 1줄에 1개씩 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/9bbd5d02-4c54-4931-9bd8-b71ab7b3a9bd">
</div>

4. 1단계 : 문제 분석하기
   - 최소 공배수는 A와 B가 주어졌을 때, A * B / 최대 공약수를 계산해 구할 수 있음
   - 결국, 이 문제는 유클리드 호제법을 이용해 최대 공약수를 구한 후 두수의 곱을 최대 공약수로 나눠주는 것으로 해결 가능

5. 2단계
   - 유클리드 호제법을 이용해 A, B의 최대 공약수 구하기
<div align="center">
<img src="https://github.com/user-attachments/assets/7d114bb4-34fc-45fb-bc45-718ad18ccb48">
</div>

   - 두 수의 곱을 최대 공약수로 나눈 값을 정대으로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/c3f79a7d-f810-4708-aa13-59a31ba42fab">
</div>

6. 3단계 : 슈도코드 작성하기
<div align="center">
<img src="https://github.com/user-attachments/assets/d91ed877-7c97-4ec6-bb52-9e77a22fc074">
</div>

7. 코드
```java
package numbertheory.euclideanalgorithm;

import java.util.Scanner;

public class LCD {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int t = sc.nextInt();

        for(int i = 0; i < t; i++) {
            int a = sc.nextInt();
            int b = sc.nextInt();
            int result = a * b / gcd(a, b);
            System.out.println(result);
        }
    }

    public static int gcd(int a, int b) {
        if(b == 0) {
            return a;
        } else {
            return gcd(b, a % b); // 재귀 함수 형태로 구현
        }
    }
}
```
