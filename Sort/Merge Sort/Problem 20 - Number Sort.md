-----
### 문제 20 - 수 정렬하기 (문제 2751번)
-----
1. 문제
```
N개의 수가 주어졌을 때, 이를 오름차순 정렬하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 수의 개수 N(1 ≤ N ≤ 1,000,000)
   - 2번쨰 줄에 N개의 줄에 숫자가 주어짐 : 이 수는 절댓값이 1,000,000보다 작거나 같은 정수 (수는 중복되지 않음)

3. 출력 : 1번째 줄부터 N개의 줄에 오름차순 정렬한 결과를 1줄에 1개씩 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/a6b92132-21f6-4ea8-897a-94c595d85aa0">
</div>

4. 1단계 : 문제 분석하기
   - N의 최대 범위가 1,000,000이므로 O(n log n)의 시간 복잡도로 정렬을 수행
   - 병합 정렬로 정렬을 수행한 후 결과 출력

5. 2단계
   - 정렬한 그룹을 최소 길이로 나눔 : 원본 배열 길이가 5이므로 2, 2, 1 길이로 분할
   - 이제 나눈 그룹마다 병합 정렬 : 각 그룹마다 index1, index2를 지정하여 비교하면서 정렬 용도로 선언한 tmp 배열에 병합 정렬
<div align="center">
<img src="https://github.com/user-attachments/assets/1a7488f3-91c5-430e-9368-cba257fec51a">
</div>

   - 현재의 그룹이 3개이므로, 2번쨰 / 3번쨰 그룹 병합
<div align="center">
<img src="https://github.com/user-attachments/assets/1c4a3d1b-0ebb-4f24-94b4-da4093ab424b">
</div>

   - 이어서 병합된 그룹을 대상으로 정렬 : 오른쪽 그룹을 병합할 때는 최초 비교 시 index2의 값(1)이 선택되어 뒤 세트가 모두가 사용
     + 이런 경우 남아 있는 세트(앞 세트)의 값 (2, 3)을 차례대로 적어줌

   - 마지막 정렬 : 1, 2, 3, 4, 5 순서로 깔끔하게 정렬
<div align="center">
<img src="https://github.com/user-attachments/assets/fee22db5-2528-4f72-aa95-15b410cb977a">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/2b5eb434-c611-4964-ba69-eb5977bbacbf">
</div>

7. 코드
```java
package sort.mergesort;

import java.io.*;

public class NumberSort2 {
    public static int[] A, tmp;
    public static long result;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());
        A = new int[N + 1];
        tmp = new int[N + 1];

        for (int i = 1; i <= N; i++) {
            A[i] = Integer.parseInt(br.readLine());
        }

        merge_sort(1, N);

        for (int i = 1; i <= N; i++) {
            bw.write(A[i] + "\n");
        }

        bw.flush();
        bw.close();
    }

    private static void merge_sort(int s, int e) {
        if(e - s < 1) {
            return;
        }

        int m = s + (e - s) / 2;

        // 재귀함수 형태로 구현
        merge_sort(s, m);
        merge_sort(m + 1, e);

        for (int i = s; i <= e; i++) {
            tmp[i] = A[i];
        }

        int k = s;
        int index1 = s;
        int index2 = m + 1;

        while (index1 <= m && index2 <= e) { // 두 그룹을 병합하는 로직
            // 양쪽 그룹의 index가 가리키는 값을 비교해 더 적은 수를 선택해 배열에 저장
            // 선택된 데이터의 index 값을 오른쪽으로 한 칸 이동

            if(tmp[index1] > tmp[index2]) {
                A[k] = tmp[index2];
                k++;
                index2++;
            } else {
                A[k] = tmp[index1];
                k++;
                index1++;
            }
        }

        // 한쪽 그룹이 모두 선택된 후 남아 있는 값 정리
        while (index1 <= m) {
            A[k] = tmp[index1];
            k++;
            index1++;
        }

        while (index2 <= e) {
            A[k] = tmp[index2];
            k++;
            index2++;
        }
    }
}
```
