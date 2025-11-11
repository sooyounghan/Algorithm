-----
### 문제 3 - 구간 합 구하기 (11659번)
-----
1. 문제
```
수 N개가 주어졌을 때, i번째 수에서 j번쨰 수까지의 합을 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 수의 개수 N(1 ≤ N 100,000), 합을 구해야 하는 횟수 M(1 ≤ M ≤ 100,000)
   - 2번쨰 줄에 N개의 수가 주어짐
   - 각 수는 1,000보다 작거나 같은 자연수
   - 3번쨰 줄부터는 M개의 줄에 합을 구해야 하는 구간 i와 j가 주어짐

3. 출력 : 총 M개의 줄에 입력으로 주어진 i번째 수에서 j번쨰 수까지의 합을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/96f7b9f3-9da7-4035-a365-23e9b76030c4">
</div>

4. 1단계 : 문제 분석하기
   - 문제에서 수의 개수와 합을 구해야 하는 횟수는 최대 100,000으로, 구간마다 매번 합을 계산하면 0.5초 안에 모든 계산을 끝낼 수 없음 (구간 합을 매번 계산한다면 최악의 경우 1억 회 이상 연산을 수행하게 되어 1초 이상 수행 시간이 필요)
   - 이럴 때 바로 구간합을 이용해야 함

5. 2단계
   - N개의 수를 입력받음과 동시에 합 배열 생성
<div align="center">
<img src="https://github.com/user-attachments/assets/0c9f12b3-583a-497f-bbbe-9de68e78d62f">
</div>

   - 구간 i ~ j가 주어지면 구간 합을 구하는 공식 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/5021d969-19c1-48d3-a327-59cf3f682bd2">
</div>

6. 3단계 : 슈도코드 작성하기
<div align="center">
<img src="https://github.com/user-attachments/assets/088625ea-ae30-4097-9272-2a060096095a">
</div>

7. 코드
```java
package dataStructure.prefixsum;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class PrefixSum {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int suNo = Integer.parseInt(st.nextToken());
        int quizNo = Integer.parseInt(st.nextToken());

        long[] S = new long[suNo + 1]; // 인덱스를 1부터 시작하도록 설정하므로 크기는 suNo + 1

        st = new StringTokenizer(br.readLine());
        for (int i = 1; i <= suNo; i++) { // 처리하기 쉽도록 인덱스 1부터 시작
            S[i] = S[i - 1] + Integer.parseInt(st.nextToken());
        }

        for (int q = 0; q < quizNo; q++) {
            st = new StringTokenizer(br.readLine());
            int i = Integer.parseInt(st.nextToken());
            int j = Integer.parseInt(st.nextToken());

            System.out.println(S[j] - S[i - 1]);
        }
    }
}
```
