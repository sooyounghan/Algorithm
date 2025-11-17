-----
### 문제 83 - 조약돌 꺼내기 (문제 13251번)
-----
1. 문제
```
효빈이의 비밀 박스에는 조약돌이 N개 들어 있다.
조약돌의 색상은 1부터 M까지 중 1개다.

비밀 박스에서 조약돌을 랜덤하게 K개 뽑았을 때 뽑은 조약돌이 모든 같은 색일 확률을 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 M(1 ≤ M ≤ 50)이 주어짐
   - 2번쨰 줄에 각 색상의 조약돌이 몇 개 있는지 주어짐 : 각 색상의 조약돌 개수는 1보다 크거나 같고, 50보다 작거나 같은 자연수
   - 3번째 줄에는 K가 주어짐 (1 ≤ K ≤ N)

3. 출력 : 1번째 줄에 뽑은 조약돌이 모든 같은 색일 확률을 출력 (정답과의 절대 및 상대 오차는 $10^{-9}$까지 허용
<div align="center">
<img src="https://github.com/user-attachments/assets/f5f2bdf4-42e8-4936-8683-c330101db63a">
</div>

4. 1단계 : 문제 붆석하기
   - 색깔별 조약돌의 개수에서 K개를 뽑을 수 있는 경우의 수를 구한 후, 전체 돌에 대해 K개를 뽑는 경우의 수로 나누면 해결 가능
   - 단순하게 확률식을 세워서도 계산 가능

5. 2단계
   - 색깔별 조약돌의 개수를 배열 D에 저장하고, 전체 조약돌 개수를 변수 T에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/cc5bcab9-22f9-426e-85fb-6df659a4eb7d">
</div>

   - 한 색깔의 조약돌만 뽑을 확률을 색깔별로 모두 구함 : 입력에서 K = 2이므로 2번 뽑을 동안 같은 색이 나올 확률을 구하면 됨
<div align="center">
<img src="https://github.com/user-attachments/assets/f46a4b15-889c-4518-a581-7638f74ec2fd">
</div>

   - 각각의 확률을 더해 정답으로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/bf5150f8-36d3-410f-8871-414671181243">
</div>

6. 3단계
<div align="center">
<img src="https://github.com/user-attachments/assets/31fbdc7a-15a8-41eb-b54c-78f65a91f8dd">
</div>

7. 코드
```java
package combination;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class TakeoutPebble {
    public static void main(String[] args) throws IOException {
        int M, K, T;

        int D[] = new int[51];
        double probability[] = new double[51];
        double ans;

        T = 0;

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        M = Integer.parseInt(st.nextToken());
        st = new StringTokenizer(br.readLine());

        for (int i = 0; i < M; i++) {
            D[i] = Integer.parseInt(st.nextToken());
            T += D[i];
        }

        st = new StringTokenizer(br.readLine());
        K = Integer.parseInt(st.nextToken());
        ans = 0.0;

        for (int i = 0; i < M; i++) {
            if(D[i] >= K) {
                probability[i] = 1.0;

                for (int k = 0; k < K; k++) {
                    probability[i] *= (double) (D[i] - k) / (T - k);
                }
            }
            ans += probability[i];
        }
        System.out.println(ans);
    }
}
```
