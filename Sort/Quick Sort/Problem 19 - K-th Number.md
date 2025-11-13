-----
### 문제 19 - K번쨰 수 구하기 (문제 11004번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/17fdaacf-10af-4ea0-8496-434375c5c096">
</div>

2. 입력
   - 1번쨰 줄에 N(1 ≤ N ≤ 5,000,000)과 K(1 ≤ K ≤ N)
   - 2번쨰 줄에 $A_{1}, A_{2}, ..., A_{N}$이 주어짐 ($-10^{9}$ ≤ $A_{i}$ ≤ $10^{9}$)

3. 출력 : A를 정렬했을 때 앞에서부터 K번째에 있는 수를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/604adcb8-9bed-404d-ba92-b2639e3e302d">
</div>

4. 1단계 : 문제 분석하기
   - N의 최대 범위가 5,000,000이므로 O(n log n)의 시간 복잡도로 정렬을 수행
   - 퀵 정렬을 구현해 주어진 수를 오름차순 정렬하고, K번쨰 수 출력
   - 단, 이 문제는 시간 복잡도가 민감하므로 퀵 정렬 알고리즘에서 K번째 수를 좀 더 빨리 구하기 위한 아이디어를 먼저 고민
   - 퀵 정렬 알고리즘을 구현하려면 먼저 pivot을 지정해야 하는데, 어떤 값을 pivot으로 정하면 K번째 수를 더 빨리 구할 수 있을지 생각해야 함
   - pivot을 정하는 방법
     + pivot == K : K번쨰 수를 찾은 것이므로 알고리즘 종료
     + pivot > K : pivot의 왼쪽 부분에 K가 있으므로 왼쪽(S ~ pivot - 1)만 정렬 수행
     + pivot < K : pivot의 오른쪽 부분에 K가 있으므로 오른쪽(pivot + 1 ~ E)만 정렬 수행

   - 데이터가 대부분 정렬되어 있는 경우 앞쪽에 있는 수를 pivot으로 선택하면 연산이 많아지므로, 배열의 중간 위치를 pivot으로 설정

5. 2단계
   - 중간 위치를 pivot으로 설정한 다음, 맨 앞에 있는 값과 swap
     + pivot을 맨 앞으로 옮기는 이유는 i, j의 이동을 편하게 하기 위함
     + 이어서 i와 j를 pivot을 제외한 그룹에서 왼쪽, 오른쪽 끝으로 정함
<div align="center">
<img src="https://github.com/user-attachments/assets/63f658e9-85ca-4486-9d0f-28788c526760">
</div>

   - 우선 j를 이동
     + j가 pivot보다 크면 j-- 연산을 반복하고, 그 결과는 j는 1에 위치
     + j를 이동한 후에 i가 pivot보다 작으면서 i보다 j가 크면 i++ 연산을 반복
     + 현재의 경우 i와 j의 위치가 같으므로 i는 이동하지 않음
<div align="center">
<img src="https://github.com/user-attachments/assets/250247e7-8d64-4a8d-a702-365aa4892ba8">
</div>

   - pivot을 두 집합을 나눠 주는 위치, 즉, i와 j가 만난 위치와 swap
<div align="center">
<img src="https://github.com/user-attachments/assets/126d3ca7-eb3d-48c7-878f-163eeb67a6bf">
</div>

   - K = 2이므로, 이제 더 이상 정렬하지 않고, A[2]를 출력

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/06000457-e260-4918-9a4b-8d3d686d266f">
</div>

7. 코드
```java
package sort.quicksort;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class KthNumber {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

        st = new StringTokenizer(br.readLine());

        int[] A = new int[N];

        for (int i = 0; i < N; i++) {
            A[i] = Integer.parseInt(st.nextToken());
        }

        quickSort(A, 0, N - 1, K - 1);
        System.out.println(A[K - 1]);
    }

    public static void quickSort(int[] A, int S, int E, int K) {
        if(S < E) {
            int pivot = partition(A, S, E);

            if(pivot == K) { // K번쨰 수가 pivot이면 더 이상 구할 필요가 없음
                return;
            } else if (K < pivot) { // K가 pivot보다 작으면 왼쪽 그룹만 정렬 수행
                quickSort(A, S, pivot - 1, K);
            } else { // K가 pivot보다 크면 오른쪽 그룹만 정렬 수행
                quickSort(A, S, pivot + 1, E);
            }
        }
    }

    public static int partition(int[] A, int S, int E) {
        if(S + 1 == E) {
            if(A[S] > A[E]) {
                swap(A, S, E);
            }
            return E;
        }

        int M = (S + E) / 2;
        swap(A, S, M); // 중앙값을 첫 번째 요소로 이동

        int pivot = A[S];

        int i = S + 1;
        int j = E;

        while(i <= j) {
            while (j >= S + 1 && pivot < A[j]) { // 피벗보다 작은 수가 나올 때 까지 j--
                j--;
            }

            while (i <= E && pivot > A[i]) { // 피벗보다 큰 수가 나올 때까지 i++
                i++;
            }

            if (i <= j) {
                swap(A, i++, j--);
            }
        }
            // 피벗 데이터를 나눠진 두 그룹 경계 index에 저장
            A[S] = A[j];
            A[j] = pivot;
            return j;
    }

    public static void swap(int[] A, int i, int j) {
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }
}
```
