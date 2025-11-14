-----
### 문제 44 - 오일러 피 함수 구현하기 (문제 11689번)
-----
1. 문제
```
자연수 n이 주어졌을 때 GCD(n, k) = 1 (1 ≤ k ≤ n)을 만족하는 자연수의 개수를 구하는 프로그램을 작성하시오.
```

2. 입력 : 1번쨰 줄에 자연수 n (1 ≤ n ≤ $10^{12}$)이 주어짐
3. 출력 : GCD(n, k) = 1 (1 ≤ k ≤ n)을 만족하는 자연수의 개수를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/d8f4aa76-ac63-4737-adea-fc1b26c7172f">
</div>

4. 1단계 : 문제 분석하기
   - 문제에서 요구하는 GCD(n, k) = 1을 만족하는 자연수의 개수가 바로 오일러 피 함수의 정의

5. 2단계
   - 서로소의 개수를 표현하는 변수 result와 현재 소인수 구성을 표시하는 변수 n을 선언 : 예제 입력 4의 경우 변수 초기화는 n = 45, result = 45
   - 오일러 피 핵심 이론 부분을 참고해 2 ~ N의 제곱근까지 탐색하면서 소인수일 때 result = result - (result / 소인수) 연산으로 result 값을 업데이트
     + 이 때, n에서 이 소인수는 나누기 연산으로 삭제
<div align="center">
<img src="https://github.com/user-attachments/assets/89aed3d5-9417-4677-b8df-6ac07f3ff4e0">
</div>

   - 반복문 종료 후 현재 n이 1보다 크면 n이 마지막 소인수라는 뜻 : result = resul - (result / n) 연산으로 result 값을 마지막으로 업데이트 한 후 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/3d592a95-06d3-4d6b-9eeb-981b69d961a2">
</div>

6. 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/0f6dd75a-c2dd-4eb5-b710-f706804c0e2c">
</div>

7. 코드
```java
package numbertheory.eulerphi;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class EulerPhiFunction {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        long n = Long.parseLong(br.readLine());
        long result = n;

        for (long p = 2; p <= Math.sqrt(n); p++) { // 제곱근까지만 진행
            if (n % p == 0) { // p가 소인수인지 확인
                result = result - result / p; // 결과값 업데이트 하기

                while (n % p == 0) { // 2^7 * 11이라면 2를 없애고 11만 남김
                    n /= p;
                }
            }
        }
        if(n > 1) { // 아직 소인수 구성이 남아있을 때
            result = result - result / n; // 반복문에서 제곱근까지만 탐색했으므로 1개의 소인수가 누락된 케이스
        }
        System.out.println(result);
    }
}
```
