-----
### 문제 87 - 정수를 1로 만들기 (문제 1463번)
-----
1. 문제
```
정수 X에 사용할 수 있는 연산은 다음 3가지다.
   1. X가 3으로 나누어떨어지면 3으로 나눈다.
   2. X가 2로 나누어떨어지면 2로 나눈다.
   3. 1을 뺀다.

정수 N이 주어졌을 때 위와 같은 연산 3개를 적절히 사용해 1을 만들려고 한다.
연산을 사용하는 횟수의 최솟값을 출력하시오.
```

2. 입력 : 1번쨰 줄에 1보다 크거나 같고, $10^{6}$보다 작거나 같은 정수 N이 주어짐
3. 출력 : 1번째 줄에 연산을 하는 횟수의 최솟값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/4b0892c5-ad0c-4c48-96b2-24ade187b93b">
</div>

4. 1단계 : 문제 분석하기 - 사용할 수 있는 3가지 연산을 바텀-업 방식으로 구현할 수 있는지 연습하는 문제

5. 2단계
   - 점화식의 형태와 의미
<div align="center">
<img src="https://github.com/user-attachments/assets/891f9ad8-f791-4161-bb3a-a91937898862">
</div>

   - 점화식 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/f4f97e7a-e276-4114-9b7e-3fdd548d9e54">
</div>

   - 점화식을 이용해 D 배열 완성
<div align="center">
<img src="https://github.com/user-attachments/assets/f0008900-aafe-4021-878c-2ac4fd2fd71c">
</div>

   - D[N]을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/3827de75-9fd0-4054-b794-f2b6acbd9446">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/a3c13f94-1261-412e-ab52-5b784647b0f8">
</div>

7. 코드
```java
package dynamicprogramming;

import java.util.Scanner;

public class Make1Integer {
    public static int N;
    public static int[] D;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        N = sc.nextInt();
        D = new int[N + 1];

        D[1] = 0;

        for(int i = 2; i <= N; i++) {
            D[i] = D[i - 1] + 1;
            if(i % 2 == 0) {
                D[i] = Math.min(D[i], D[i / 2] + 1);
            }

            if(i % 3 == 0) {
                D[i] = Math.min(D[i], D[i / 3] + 1);
            }
        }
        System.out.println(D[N]);
    }
}
```
