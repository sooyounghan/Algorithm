-----
### 문제 84 - 순열의 순서 구하기 (문제 1722번)
-----
1. 문제
```
1부터 N까지의 수를 임의로 배열한 순열의 경우의 수는 N!이다.
임의의 순열은 영문 사전의 정렬 방식과 비슷하게 정렬된다고 하자.

예를 들어, N = 3이면, {1, 2, 3}, {1, 3, 2}, {2, 1, 3}, {2, 3, 1}, {3, 1, 2}, {3, 2, 1}의 순서로 정렬된다.

N이 주어지면 다음 두 소문제 중 1개를 푸는 프로그램을 작성해보자.
  - 소문제 1 : K가 주어지면 K번째 순열을 구한다.
  - 소문제 2 : 임의의 순열이 주어지면 이 순열이 몇 번째 순열인지 구한다.
```

2. 입력
   - 1번쨰 줄에 순열의 자릿수 N(1 ≤ N ≤ 20)이 주어짐
   - 2번쨰 줄의 1번쨰 수는 소문제 번호
     + 1일 때는 K를 입력받음
     + 2일 때 임의의 순열을 나타내는 N개의 수를 입력받음 : 여기서 N개의 수는 1에서 N까지의 정수가 한 번씩만 나타남

3. 출력 : 주어진 소문제와 관련된 정답 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/c07efbd6-2a82-40dc-9931-d23546e101f1">
</div>

4. 1단계 : 문제 분석하기
   - 조합 문제와는 다르게 순열의 개념을 알아야 함 : 순열은 순서가 다르면 다른 경우의 수로 인정
     + N자리로 만들 수 있는 순열의 경우의 수를 구해야 하는 것이 문제의 핵심
     + 4자리로 표현되는 모든 경우의 수를 구하는 예제 : 가장 먼저 각 자리에서 사용할 수 있는 경우의 수를 구함
<div align="center">
<img src="https://github.com/user-attachments/assets/b3e8df26-3844-4bec-9d74-0c386eb705d0">
</div>

   - 각 자리에서 구한 경우의 수를 모두 곱하면 모든 경우의 수가 나옴 : 4자리로 표현되는 모든 경우의 수는 4 * 3 * 2 * 1 = 4! = 24
   - 이를 일반화하면 N자리로 만들 수 있는 순열의 모든 경우의 수는 N!

5. 2단계
   - 자릿수에 따른 순열의 경우의 수를 1부터 N자리까지 미리 계산
<div align="center">
<img src="https://github.com/user-attachments/assets/518fa409-9576-41aa-9d8e-902a71691651">
</div>

   - 소문제 1 해결 : 예제 1을 이용해 K번째 순열 출력
     + 주어진 값(K)과 현재 자리(N) - 1에서 만들 수 있는 경우의 수를 비교
     + 1번쨰 과정에서 K가 더 작아질 때까지 경우의 수를 배수(cnt)로 증가시킴 (순열의 순서를 1씩 늘림)
     + K가 더작아지면, 순열에 값을 저장하고 K를 K - (경우의수 * (cnt - 1))로 업데이트
     + 순열이 완성될 때까지 세 과정을 반복하고 완료된 순열을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/523020d9-b0d5-48aa-a085-b2b196dca923">
</div>

   - 소문제 2 해결 : 예제 2를 이용해 입력된 순열의 순서 K를 구하기
     + N자리 순열의 숫자를 받아 몇 번째 순서의 숫자인지 확인 (현재 숫자 - 자기보다 앞 숫자 중 이미 사용한 숫자)
     + 해당 순서 * (현재 자리 - 첫 번쨰 과정에서 만들 수 있는 순열의 개수)를 K에 더함
     + 모든 자릿수에 관해 세 과정을 반복한 후 K값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/d7ae99d4-aa56-4617-970c-c06f25776e8c">
</div>

   - 과정 이해
     + 첫 번째 과정 : 남아 있는 숫자 = {1, 3, 2, 4}이고, 현재 숫자 = 1이므로 1보다 작은 숫자는 없으므로, 앞에 오는 순열이 없고, K 변화 없음 (K = 1)
     + 두 번째 과정 :  이해 두 번째 자리는 3이며, 남은 숫자 = {3, 2, 4}이고, 현재 숫자 = 3이므로 3보다 작은 숫자 2 하나이므로, 즉, 이 자리에서 “2가 첫 번째, 3이 두 번째”이므로 3은 2번째 작은 값
        * (현재 자리에서 작은 숫자 개수) × (남은 자리로 만들 수 있는 순열 수) : 여기서 남은 자리는 2자리이므로 2! = 2
     + 세 번째 과정 : 남은 숫자 = {2, 4}이며, 2보다 작은 숫자는 없으므로, K 변화 없으므로 K = 3
     + 네 번째 과정 : 남은 숫자 = {4}이며, 4보다 작은 숫자는 없으므로, K 변화 없으므로, K = 3
     + 정리 : 순열 1 3 2 4는 전체 순열 중 3번째 순서

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/16458314-ed9f-4272-ac7f-5a2ef43d0b6f">
</div>

7. 코드
```java
package combination;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class OrderSequence {
    public static void main(String[] args) throws IOException {
        int N, Q;

        long[] F = new long[21];
        int[] S = new int[21];
        boolean[] visited = new boolean[21];

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        st = new StringTokenizer(br.readLine());
        Q = Integer.parseInt(st.nextToken());

        F[0] = 1;
        for(int i = 1; i <= N; i++) { // 팩토리얼 초기화 : 각 자릿수에서 만들 수 있는 경우의 수
            F[i] = F[i - 1] * i;
        }

        if (Q == 1) {
            long K = Long.parseLong(st.nextToken());

            for(int i = 1; i <= N; i++) {
                for(int j = 1, cnt = 1; j <= N; j++) {
                    if(visited[j]) {
                        continue; // 이미 사용한 숫자는 사용할 수 없음
                    }

                    if(K <= cnt * F[N - i]) { // 주어진 K에 따라 각 자리에 들어갈 수 있는 수 찾기
                        K -= ((cnt - 1) * F[N - i]);
                        S[i] = j;
                        visited[j] = true;
                        break;
                    }
                    cnt++;
                }
            }

            for(int i = 1; i <= N; i++) {
                System.out.print(S[i] + " ");
            }
        } else {
            long K = 1;

            for(int i = 1; i <= N; i++) {
                S[i] = Integer.parseInt(st.nextToken());

                long cnt = 0;

                for(int j = 1; j < S[i]; j++) {
                    if(visited[j] == false) {
                        cnt++; // 미사용 숫자 개수만큼 카운트
                    }
                }

                K += cnt * F[N - i]; // 자릿수에 따라 순서 더하기
                visited[S[i]] = true;
            }

            System.out.print(K);
        }
    }
}
```
