-----
### 문제 5 - 나머지 합 구하기 (10986 번)
-----
1. 문제
```
N개의 수 A₁, A₂, ..., Aₙ이 주어졌을 때, 연속된 부분의 합이 M으로 나누어떨어지는 구간의 개수를 구하는 프로그램을 작성하시오.
즉, Aᵢ + ... Aⱼ (i ≤ j)의 합이 M으로 나누어떨어지는 (i, j) 쌍의 개수를 구하시오.
```

2. 입력
   - 1번쨰 줄에 N과 M(1 ≤ N, $10^{6}$, 2 ≤ M ≤ $10^{3}$)이 주어짐
   - 2번째 줄에 N개의 수 A₁, A₂, ..., Aₙ이 주어짐 (0 ≤ Aᵢ ≤ $10^{9}$)

3. 출력 : 1번째 줄에 연속된 부분의 합이 M으로 나누어떨어지는 구간의 개수 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/5ad6b310-f23d-4793-be46-506cb283edcb">
</div>

4. 1단계 : 문제 분석하기
   - N의 최댓값이 $10^{6}$이라 연산량이 작게 느껴질 수 있음 : $10^{6}$개의 수에 대해 모든 구간 합을 구해야하므로 1초 안에 연산하기 어려우므로, 구간 합 배열을 이용해야 함
   - 나머지 합 문제 풀이의 핵심 아이디어
     + (A + B) % C는 ((A % C) + (B % C)) % C와 같음 : 즉, 다시 말해 특정 구간 수들의 나머지 연산을 더해 나머지 연산을 한 값과 이 구간 합에 나머지 연산을 한 값은 동일
     + 구간 합 배열을 이용한 식 S[j] - S[i]는 원본 배열의 i + 1부터 j까지 구간 합
     + S[j] % M의 값과 S[i] % M의 값이 같다면 (S[j] - S[i]) % M은 0 : 즉, 구간 합 배열의 원소를 M으로 나눈 나머지로 업데이트하고, S[j]와 S[i]가 같은 (i, j) 쌍을 찾으면 원본 배열에서 i + 1부터 j까지의 구간 합이 M으로 나누어떨어지는 것을 알 수 있음

5. 2단계
   - A 배열의 합 배열 S를 생성
<div align="center">
<img src="https://github.com/user-attachments/assets/a678657d-a70d-466b-b36a-dcef4a37f9e6">
</div>

   - 합 배열 S의 모든 값에 대해 M으로 나머지 연산을 수행해 값을 업데이트
<div align="center">
<img src="https://github.com/user-attachments/assets/57f8b352-d571-4393-b6e1-1a46aeb1d1a4">
</div>

   - 우선 변경된 합 배열에서 원소 값이 0인 개수만 세어 정답에 더함 : 변경된 합 배열 원소 값이 0이라는 뜻은 원본 배열의 0부터 i까지 구간 합이 이미 M으로 나누어떨어진다느 뜻이기 때문임
<div align="center">
<img src="https://github.com/user-attachments/assets/acbb629d-eba3-4ba3-a019-478d25c7b44b">
</div>

   - 이제 변경된 합 배열에서 원소 값이 같은 인덱스의 개수, 즉 나머지 값이 같은 합 배열의 개수를 셈
     + 변경된 합 배열에서 원소 값이 같은 2개의 원소를 뽑는 모든 경우의 수를 구하여 정답에 더하면 됨
     + 위 예에서는 0이 3개, 1이 2개이므로, ${}^{3}C_{2}, {}^{3}C_{1}$로 경우의 수를 구하여 더하면 됨
<div align="center">
<img src="https://github.com/user-attachments/assets/807dea6c-176c-4d5f-b461-0d660e410958">
</div>

6. 슈도코드 작성하기
<div align="center">
<img src="https://github.com/user-attachments/assets/823620fe-2112-4f85-ab7d-4bf45a293181">
</div>

7. 코드
```java
package dataStructure.prefixsum;

import java.util.Scanner;

public class RemainderSum {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int M = sc.nextInt();

        long[] S = new long[N];
        long[] C = new long[M];

        long answer = 0;
        S[0] = sc.nextInt();

        for (int i = 1; i < N; i++) { // 수열 합 배열 만들기
            S[i] = S[i - 1] + sc.nextInt();
        }

        for (int i = 0; i < N; i++) { // 합 배열의 모든 값에 % 연산 수행
            int remainder = (int) (S[i] % M);

            // 0 ~ i 까지 구간 합 자체가 0일 때 정답에 더하기 = (i, i)
            if(remainder == 0) answer++;

            // 나머지 같은 인덱스 개수 카운팅
            C[remainder]++;
        }

        for (int i = 0; i < M; i++) {
            if(C[i] > 1) { // 배열 C는 카운팅한 배열이므로 최소 2개 이상이어야 조합 가능
                // 나머지가 같은 인덱스 중 2개를 뽑는 경우의 수를 더하기
                answer = answer + (C[i] * (C[i] - 1) / 2);
            }
        }

        System.out.println(answer);
    }
}
```
