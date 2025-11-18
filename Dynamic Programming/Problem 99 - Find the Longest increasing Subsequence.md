-----
### 문제 99 - 가장 길게 증가하는 부분 수열 찾기 (문제 14003번)
-----
1. 문제
```
수열 A가 주어졌을 때, 가장 길게 증가하는 부분 수열을 구하는 프로그램을 작성하시오.

예를 들어, 수열 A = (10, 20, 10, 30, 20, 50)일 때, 가장 길게 증가하는 부분 수열은 (10, 20, 30, 50)이고, 길이는 4이다.
```

2. 입력
   - 1번쨰 줄에 수열 A의 크기 N (1 ≤ N ≤ 1,000,000)
   - 2번째 줄에 수열 A를 이루고 있는 $A_{i}$가 주어짐 (-1,000,000,00 ≤ A ≤ 1,000,000,000)

3. 출력
   - 1번째 줄에 수열 A의 가장 길게 증가하는 부분 수열의 길이 출력
   - 2번째 줄에 정답이 될 수 있는 가장 길게 증가하는 부분 수열 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/724e1fc0-9367-478d-bcc7-9ca0f201e88e">
</div>

4. 1단계 : 문제 분석하기
   - 가장 길게 증가하는 부분 수열(최장 증가 수열)의 점화식
<div align="center">
<img src="https://github.com/user-attachments/assets/7a926032-42a8-4e51-a7d0-c2b8e89d47d9">
</div>

   - 부분 문제를 이용해 전체 문제를 풀이하려면 i의 값이 부분 문제의 핵심이 되도록 정의해야 하므로 D[i]를 단순히 0 ~ i까지 최장 증가 수열 길이가 아닌 0 ~ i까지 i를 포함하는 증가 수열의 길이로 정의하는 것이 중요
   - 단, 문제에서 최댓값이 1,000,000이므로 크기 때문에 시간 복잡도를 고려해 풀이를 설계

5. 2단계
   - 점화식 도출
     + A[i]를 i번쨰 수열의 값이라 정의 : D[k]는 A[i] > A[k]를 만족하는 최대 길이의 집합
     + 즉, A[i]보다 작은 값을 지니고 있는 수열의 최장 증가 수열의 길이들 중 최댓값을 찾아 해당 값에 +1을 한 값을 D[i]에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/83de3cca-1b97-4687-9bdc-56bbc9966942">
</div>

   - 점화식을 이용해 D 배열의 값을 저장
     + 이 때, 자신보다 작은 값을 지니고 있는 최장 증가 수열 길이를 찾기 위해 B 배열을 만들어 현재 가장 유리한 수열을 실시간 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/e8559c59-fe81-459f-81b0-e60a9621dc67">
</div>

   - D 배열을 이용해 정답을 출력
     + 먼저 뒤에서 부터 탐색해 최댓값(5)과 동일한 값을 가지는 최초 index의 A[]값을 출력
     + 그리고 값을 1만큼 감소시키고 A[]의 값이 0이 될 때까지 반복
<div align="center">
<img src="https://github.com/user-attachments/assets/ba16b06b-8da0-456a-980b-99283ae6991b">
</div>

   - 시간 복잡도를 줄이기 위해 자신이 들어갈 수 있는 위치를 찾는 이진 탐색을 이용해 구현

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/86ce8884-5f22-45cd-bbdd-3492ef27427b">
</div>

7. 코드
```java
package dynamicprogramming;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class LongestSubsequence {
    public static int N, maxLength;
    public static int B[] = new int[1000001];
    public static int A[] = new int[1000001];
    public static int D[] = new int[1000001];
    public static int ans[] = new int[1000001];

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        N = Integer.parseInt(br.readLine());

        StringTokenizer st = new StringTokenizer(br.readLine());

        for(int i = 1; i <= N; i++) {
            A[i] = Integer.parseInt(st.nextToken());
        }

        int index;
        B[++maxLength] = A[1];
        D[1] = 1;

        for(int i = 2; i <= N; i++) {
            if (B[maxLength] < A[i]) { // 가장 마지막 수열보다 현재 수열이 클 때
                B[++maxLength] = A[i];
                D[i] = maxLength;
            } else {
                // B 배열에서 A[i]보다 처음으로 크거나 같아지는 원소의 index 찾기
                index = binarysearch(1, maxLength, A[i]);
                B[index] = A[i];
                D[i] = index;
            }
        }

        System.out.println(maxLength); // 가장 길고, 증가하는 부분의 수열 길이 출력

        index = maxLength;
        int x = B[maxLength] + 1;

        for(int i = N; i >= 1; i--) { // 뒤에서부터 탐색하면서 정답 수열 저장
            if(D[i] == index && A[i] < x) {
                ans[index] = A[i];
                x = A[i];
                index--;
            }
        }

        for (int i = 1; i <= maxLength; i++) {
            System.out.print(ans[i] + " ");
        }
    }

    // 현재 수열이 들어갈 수 있는 위치를 빠르게 찾기 위한 이진 탐색 구현
    public static int binarysearch(int l, int r, int now) {
        int mid;

        while (l < r) {
            mid = (l + r) / 2;

            if (B[mid] < now) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return l;
    }
}
```
