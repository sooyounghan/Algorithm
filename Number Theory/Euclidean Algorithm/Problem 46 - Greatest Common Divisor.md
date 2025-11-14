-----
### 문제 46 - 최대 공약수 구하기 (문제 1850번)
-----
1. 문제
```
모든 자리가 1로만 이뤄진 A와 B가 주어져 있다.
이 때 A와 B의 최대 공약수를 구하는 프로그램을 작성하시오.
예를 들어 A가 111이고, B가 1111일 때, A와 B의 최대 공약수는 1이다.
A가 111이고, B가 1111111일 경우에는 최대 공약수가 111이다.
```

2. 입력
   - 1번째 줄에는 두 자연수 A와 B를 이루는 1의 개수가 주어짐
   - 입력되는 수는 $2^{63}$보다 작은 자연수

3. 출력 : 1번째 줄에 A와 B의 최대 공약수 출력 (정답은 1,000만 자리를 넘어가지 않음)
<div align="center">
<img src="https://github.com/user-attachments/assets/f96a91e5-52fc-455d-9175-66672d50b4fd">
</div>

4. 1단계 : 문제 분석하기
   - 예제 입력 3과 같이 입력값이 크면 단순한 방법으로 최소 공배수를 찾을 수 없음
   - 예제의 규칙
     + 💡 수의 길이를 나타내는 두 수의 최대 공약수는 A와 B의 최대 공약수의 길이를 나타냄
     + 즉, 3, 6의 최대 공약수 3은 A(111)와 B(111111)의 최대 공약수(111)의 길이를 나타냄

5. 2단계
   - 유클리드 호제법을 이용해 A, B의 최대 공약수 구하기
<div align="center">
<img src="https://github.com/user-attachments/assets/a668d29c-2008-442c-9760-86f5c1975966">
</div>

   - 공약수의 길이만큼 1을 반복해 출력 : 일반적인 출력을 수행하면 시간 초과가 발생할 수 있으므로 BufferedWriter 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/a3a2115b-a197-4745-aaf7-3e5cc75c2b42">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/420439e7-085c-4b14-b279-5820d845dd2e">
</div>

7. 코드
```java
package numbertheory.euclideanalgorithm;

import java.io.*;
import java.util.Scanner;

public class GCD {
    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);

        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        
        long a = sc.nextLong();
        long b = sc.nextLong();
        long result = gcd(a, b);
        
        while(result > 0) {
            bw.write("1");
            result--;
        }
        
        bw.flush();
        bw.close();
    }

    public static long gcd(long a, long b) {
        if(b == 0) {
            return a;
        } else {
            return gcd(b, a % b);
        }
    }
}
```
