-----
### 문제 21 - 버블 정렬 프로그램 2 (문제 1517번)
-----
1. 문제
```
버블 정렬은 서로 인접해 있는 두 수를 바꾸면서 정렬하는 방법이다.
예를 들어 수열이 3, 2, 1이었다고 가정해보자. 이 때는 인접해 있는 3, 2가 바뀌어야 하므로 2, 3, 1이 된다.
그 다음은 3, 1이 바뀌어야 하므로 2, 1, 3이 된다. 그 다음에는 2, 1이 바뀌어야 하므로 1, 2, 3이 된다.
그러면 더 이상 바꿀 수 없으므로, 정렬이 완료된다.

N개의 수로 이뤄진 수열 A[1], A[2], ..., A[N]이 있다. 이 수열로 버블 정렬을 수행할 때 swap이 총 몇 번 발생하는지 알아내는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 N(1 ≤ N ≤ 500,000)
   - 2번째 줄에 N개의 정수로 A[1], A[2], ..., A[N]이 주어짐
   - 각각의 A[i]는 0 ≤ |A[i]| ≤ 1,000,000,000을 만족

3. 출력 : 1번째 줄에 swap 횟수 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/d59c551e-8145-47f3-ab6f-ffe11ba4c78e">
</div>

4. 1단계 : 문제 분석하기
   - N의 최대 범위가 500,000이므로 O(n log n)의 시간 복잡도로 정렬을 수행하면 됨 : 병합 정렬로 정렬을 수행한 후 결과 출력
   - 제목은 버블 정렬이지만, N의 ㅊ최대 범위가 500,000이므로 버블 정렬을 사용하면 제한 시간 초과 : 즉, 이 문제는 버블 정렬이 아닌 O(n log n)의 시간 복잡도를 가진 병합 정렬을 사용
   - 병합 정렬을 이해했다면, 두 그룹을 병합하는 과정에서 버블 정렬의 swap이 포함되어 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/53c74c40-ffb1-4850-ae2b-f79284c35613">
</div>

   - 두 그룹을 병합 정렬하는 과정에서 뒤쪽 그룹 5가 앞쪽 그룹의 24, 43, 42, 60의 앞에 놓임 : 이는 버블 정렬에서 swap을 4번해야 볼 수 있는 효과
   - 45는 60보다 앞에 놓이므로 버블 정렬에서 swap을 1번 한 것과 동일

5. 2단계
   - 병합 정렬은 동일하게 진행하되, 정렬 과정에서 index가 이동한 거리를 result에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/44d9e927-0692-447d-9448-61baeef4e3d9">
</div>

   - 그룹을 정렬하는 과정에서 원소 앞으로 이동한 거리만큼 result에 더함
   - 예를 들어 (3, 2), (8, 1), (7, 4), (5, 6) 그룹은 1, 2, 3번째 그룹에서 2, 1, 4 원소만 이동하므로 result에 총 3을 더함
   - (2, 3, 1, 8), (4, 7, 5, 6) 그룹은 1이 2칸, 5, 6이 각각 1칸씩 이동하므로 result에 4를 더함
   - 마지막으로 (1, 2, 3, 8, 4, 5, 6, 7)은 4, 5, 6, 7이 1칸씩 이동하므로 result에 4를 더함

6. 3단계 : 슈도코드 작성
   - swap 횟수를 세기 위한 로직만 추가
<div align="center">
<img src="https://github.com/user-attachments/assets/4a853306-61ec-4214-98c9-bdbf360c911c">
</div>

7. 코드
```java
package sort.mergesort;

import java.io.*;
import java.util.StringTokenizer;

public class BubbleSort2 {
    public static int[] A, tmp;
    public static long result;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());
        A = new int[N + 1];
        tmp = new int[N + 1];

        StringTokenizer st = new StringTokenizer(br.readLine());

        for (int i = 1; i <= N; i++) {
            A[i] = Integer.parseInt(st.nextToken());
        }

        result = 0;
        merge_sort(1, N); // 병합 정렬 수행

        System.out.println(result);
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
                result = result + index2 - k; // 뒤쪽 데이터가 작은 경우 result 업데이트
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
