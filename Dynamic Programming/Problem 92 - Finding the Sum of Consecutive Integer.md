-----
### 문제 92 - 연속된 정수의 합 구하기 (문제 13398번)
-----
1. 문제
```
n개의 정수로 이뤄진 임의의 수열이 주어진다.
이 중 연속된 몇 개의 수를 선택해 구할 수 있는 합 중 가장 큰 합을 구하려고 한다. 단, 수는 1개 이상 선택해야 한다.
또한, 수열에서 수를 1개 제거할 수 있다. (제거하지 않아도 된다.)

예를 들어, 10, -4 , 3, 1, 5, 6, -35, 12, 21, -1이라는 수열이 주어졌다고 가정해보자.
여기서 수를 제거하지 않았을 때의 정답은 12 + 21 = 33이 된다.
만약 -35를 제거한다면 수열은 10, -4, 3, 1, 5, 6, 12, 21, -1이 되고, 여기서 정답은 10 - 4 + 3 + 1 + 5 + 6 + 12 + 21 = 54가 된다.
```

2. 입력
   - 1번째 줄에 정수 n(1 ≤ n ≤ 100,000)이 주어짐
   - 2번쨰 줄에 n개의 정수로 이뤄진 수열이 주어짐
   - 수는 -1,000보다 크거나 같고, 1,000보다 작거나 같은 정수

3. 출력 : 1번째 줄에 답을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/8f4c41a2-254b-4349-8e4e-5b06a172c9cf">
</div>

4. 1단계 : 문제 분석하기
   - 동적 계획법에서 점화식을 정의할 때 가장 흔하게 하는 실수
     + 동적 계획법에서는 큰 문제를 작은 문제로 나눌 수 있고, 이러한 작은 문제들을 해결해 궁극적으로 문제에서 요구하는 큰 문제를 해결
     + 위 문자에서는 수열에서 가장 큰 합을 구하려고 할 때, 수열에서 수를 1개 제거할 수 있음 (제거 여부는 자유)
   - 아래 점화식 정의는 잘못됨 : 바로 큰 문제를 부분 문제로 나눴을 때 부분 문제는 큰 문제를 해결하기 위한 1개의 부분이 되어야 한다는 것을 위배
     + 즉, 이 정의에서 N값은 명확하지 않음
<div align="center">
<img src="https://github.com/user-attachments/assets/4fd779d7-8ad8-4607-9498-4aa429a1ac02">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/dec504d1-ea15-45a7-98fb-9f4afa5d50c4">
</div>

   - D[3]을 구하면 값이 10이 됨 : 맨 앞의 수 10을 선택한 후, 아무것도 선택하지 않는 게 가장 큰 값이기 떄문임
     + 그러나, D[N]. D[1]. D[2]의 값 모두 10이라는 것을 알 수 있음
     + 즉, D[N]에서 N값이 문제를 부분 문제로 나누는 데 적절하지 못한 정의라는 것을 알 수 있음

   - 올바른 점화식 정의
<div align="center">
<img src="https://github.com/user-attachments/assets/d417e027-1200-4f9b-aa18-19e7bc73eac8">
</div>

   - 따라서, 항상 동적 계획법에서 점화식을 정의할 때는 배열에서 인덱스가 큰 문제를 적절하게 작은 문제로 분리할 수 있는 고려해야 함

5. 2단계
   - 1개의 수를 삭제할 수 있으므로, 왼쪽 방향에서부터 인덱스르 포함한 최대 연속 합을 구하고, 오른쪽 방향에서부터 인덱스를 포함한 최대 연속 합을 한 번 더 구함
   - 양쪽에서 구한 후, L[N - 1] + R[N + 1]을 하면, N을 1개 제거한 최댓값을 구하는 효과가 있음
   - 주어진 수열을 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/35e69169-c148-43cf-b1d5-0175c3850e9b">
</div>

   - 점화식을 이용해 왼쪽, 오른쪽 방향과 관련된 인덱스를 포함한 최대 연속 합 배열을 채움
<div align="center">
<img src="https://github.com/user-attachments/assets/e39bd529-f341-4247-9cc1-9842ab878347">
</div>

   - 계산된 두 배열을 이용해 최댓값 찾기 : i번째 수를 삭제했을 때 최댓값은 L[i - 1] + R[i + 1] - 이에 따라 이 배열에서 최댓값은 6번째 수를 삭제할 때임
<div align="center">
<img src="https://github.com/user-attachments/assets/40ee79ca-330f-4613-bbf8-42a057296a91">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/b8fbfa0f-83a4-4a72-9ced-f0900b4ab4cb">
</div>

7. 코드
```java
package dynamicprogramming;

import java.io.*;
import java.util.StringTokenizer;

public class ConsecutiveNumber {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());
        StringTokenizer st = new StringTokenizer(br.readLine());

        int[] A = new int[N];

        for (int i = 0; i < N; i++) {
            A[i] = Integer.parseInt(st.nextToken());
        }

        // 오른쪽 방향으로 index를 포함한 최대 연속 합 구하기
        int[] L = new int[N];
        L[0] = A[0];
        int result = L[0];

        for (int i = 1; i < N; i++) {
            L[i] = Math.max(A[i], L[i - 1] + A[i]);
            result = Math.max(result, L[i]); // 1개도 제거하지 않았을 때, 기본 최댓값으로 저장
        }

        // 왼쪽 방향으로 index를 포함한 최대 연속 합 구하기
        int[] R = new int[N];
        R[N - 1] = A[N - 1];
        for (int i = N - 2; i >= 0; i--) {
            R[i] = Math.max(A[i], R[i + 1] + A[i]);
        }

        // L[i - 1] + R[i + 1] 2개의 구간 합 배열을 더하면 i번째 값을 제거한 효과를 얻음
        for (int i = 1; i < N - 1; i++) {
            int temp = L[i - 1] + R[i + 1];
            result = Math.max(result, temp);
        }

        bw.write(result + "\n");
        bw.flush();
        bw.close();
        br.close();
    }
}
```
