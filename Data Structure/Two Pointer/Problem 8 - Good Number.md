-----
### 문제 8 - '좋은 수' 구하기 (문제 1253번)
-----
1. 문제
```
주어진 N개의 수에서 다른 두 수의 합으로 표현되는 수가 있다면 그 수를 '좋은 수'라고 한다.
N개의 수 중 좋은 수가 몇 개인지 출력하시오.
```

2. 입력
   - 1번째 줄에 수의 개수 N(1 ≤ N ≤ 2,000)
   - 2번째 줄에 N개의 수의 값($A_{i}$)이 주어짐 (| $A_{i}$ | ≤ 1,000,000,000, $A_{i}$는 정수)

3. 출력 : 좋은 수의 개수 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/25bf2533-2518-4b58-86d6-377e257be59b">
</div>

4. 1단계 : 문제 분석하기
   - 시간 복잡도 : N의 개수가 최대 2,000이라 가정해도 좋은 수 하나를 찾는 알고리즘 시간 복잡도는 $N^{2}$보다 작아야 함
     + 만약 시간 복잡도가 $N^{2}$인 알고리즘을 사용하면 최종 시간 복잡도는 $N^{3}$이 되어 제한 시간 안에 문제를 풀 수 없음
   - 따라서, 좋은 수 하나를 찾는 알고리즘 시간 복잡도는 최소 O(n log n)이어야 함
   - 💡정렬, 투포인터 알고리즘 사용하되, 단, 정렬된 데이터에서 자기 자신을 좋은 수 만들기에 포함하면 안 됨 (이 점을 예외 처리해야 함)

5. 2단계
   - 수를 입력받아 배열에 저장한 후 정렬
<div align="center">
<img src="https://github.com/user-attachments/assets/c27998d6-04e1-48d6-aaae-17486320f639">
</div>

   - 투 포인터 i, j를 배열 A의 양쪽 끝에 위치시키고 조건에 적합한 투 포인터 이동 원칙을 활용해 탐색 수행 (판별 대상이 되는 수는 K라고 가정)
<div align="center">
<img src="https://github.com/user-attachments/assets/d4b9f838-d835-4374-b500-184745c9cbce">
</div>

   - 위 단계 배열의 모든 수에 대해 반복 : 즉, K가 N이 될 때까지 반복하여 좋은 수가 몇 개 인지 확인
<div align="center">
<img src="https://github.com/user-attachments/assets/246c7fa7-5e97-4196-b95a-17b8eb393aa5">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/7cc7f625-aedc-4c65-95f3-fcdb66c4c217">
</div>

7. 코드
```java
package dataStructure.twopointer;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class GoodNumber {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());
        int Result = 0;
        long[] A = new long[N];

        StringTokenizer st = new StringTokenizer(br.readLine());

        for (int i = 0; i < N; i++) {
            A[i] = Long.parseLong(st.nextToken());
        }

        Arrays.sort(A);

        for(int k = 0; k < N; k++) {
            long find = A[k];
            int i = 0;
            int j = N - 1;

            // 투 포인터 알고리즘
            while (i < j) {
                if (A[i] + A[j] == find) {
                    // find가 서로 다른 두 수의 합인지 체크
                    if (i != k && j != k) {
                        Result++;
                        break;
                    } else if (i == k) {
                        i++;
                    } else if (j == k) {
                        j--;
                    }
                } else if (A[i] + A[j] < find) {
                    i++;
                } else {
                    j--;
                }
            }
        }

        System.out.println(Result);
        br.close();
    }
}
```
