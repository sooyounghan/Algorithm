-----
### 문제 48 - Ax + By = C (문제 21568번)
-----
1. 문제
```
A, B, C가 주어졌을 때 Ax + By = C를 만족하면서 다음 조건을 만족하는 (x, y) 쌍을 찾으시오.
   - x, y는 정수
   - -1,000,000,000 ≤ x, y ≤ 1,000,000,000
```

2. 입력 : 1번쨰 줄에 정수 A, B, C가 주어짐
3. 출력
   - Ax + By = C를 만족하는 x, y를 공백으로 구분해 출력
   - 문제의 조건을 만족하는 (x, y)가 존재하지 않을 때는 -1을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/6ea05c73-fea1-4ea0-a0fd-bdaa9d2f385b">
</div>

4. 1단계 : 문제 분석하기
   - 확장 유클리드 호제법을 그대로 구현하면 되는 문제

5. 2단계
   - C의 값이 A와 B의 최대 공약수의 배수 형태인지 확인 : 최대 공약수의 배수 형태라면 C의 값을 최대 공약수로 변경
   - 최대 공약수의 배수 형태가 아니라면 -1을 출력한 후 프로그램 종료
<div align="center">
<img src="https://github.com/user-attachments/assets/9627aac7-8cb7-46cf-b908-63785ca19582">
</div>

   - A와 B에 관해 나머지가 0이 나올 때까지 유클리드 호제법을 수행
<div align="center">
<img src="https://github.com/user-attachments/assets/454e36f5-9671-45d0-b774-0378a9f16636">
</div>

   - 나머지가 0이 나오면 x = 1, y = 0으로 설정한 후, 2번째 과정에서 구한 몫들을 식(x = y', y = x' - y' * 몫)에 대입하면서 역순으로 계산
<div align="center">
<img src="https://github.com/user-attachments/assets/629d9a72-5493-469c-aeca-19cd90332cfe">
</div>

   - 최종으로 계산된 x, y 값에 C를 x와 y의 최대 공약수로 나눈 값을 각각 곱해 방정식의 해를 구함
<div align="center">
<img src="https://github.com/user-attachments/assets/375d1c14-0903-4503-a576-86b62a38ac62">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/2a0e3d14-fabf-46f3-8c70-f16ebc6100ee">
</div>

7. 코드
```java
package numbertheory.euclideanalgorithm;

import java.io.*;
import java.util.StringTokenizer;

public class AxByC {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int a = Integer.parseInt(st.nextToken());
        int b = Integer.parseInt(st.nextToken());
        int c = Integer.parseInt(st.nextToken());

        long gcd = gcd(a, b);

        if(c % gcd != 0) {
            System.out.println(-1);
        } else {
            int mok = (int) (c / gcd);
            long[] ret = Execute(a, b);
            System.out.println(ret[0] * mok + " " + ret[1] * mok);
        }
    }

    private static long[] Execute(long a, long b) {
        long[] ret = new long[2];

        if(b == 0) {
            ret[0] = 1; // x = 1, y = 0로 설정하고 반환
            ret[1] = 0;

            return ret;
        }

        long q = a / b;
        long[] v = Execute(b, a % b); // 재귀 형태로 유클리드 호제법 수행
        ret[0] = v[1]; // 역순으로 올라오면서 x, y 값을 계산하는 로직
        ret[1] = v[0] - v[1] * q;
        return ret;
    }

    private static long gcd(long a, long b) {
        while(b != 0) {
            long temp = a % b;
            a = b;
            b = temp;
        }

        return Math.abs(a);
    }
}
```
