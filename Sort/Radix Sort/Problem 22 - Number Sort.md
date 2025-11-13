-----
### 문제 22 - 수 정렬하기 3 (문제 10989번)
-----
1. 문제
```
N개의 수가 주어졌을 때 이를 오름차순 정렬하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 수의 개수 N (1 ≤ N ≤ 10,000,000)
   - 2번째 줄부터 N개의 숫자가 주어짐 (이 수는 10,000보다 작거나 같은 자연수)

3. 출력 : 1번쨰 줄부터 N개의 줄에 오름차순 정렬한 결과가 1줄에 1개씩 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/7438aa32-3c17-4eb6-b856-95284018cfb5">
</div>

4. 1단계 : 문제 분석하기
   - 이 문제는 N의 최대 개수가 10,000,000으로 매우 크기 때문에 O(n log n)보다 더 빠른 알고리즘이 필요
   - 문제에서 주어지는 숫자의 크기가 10,000보다 작다는 것을 바탕으로 O(kn) 시간 복잡도의 기수 정렬을 사용하면 됨

5. 2단계
   - 자릿수가 서로 다른 경우 자릿수가 적은 수 앞에 0이 채워져 있다고 생각하여 큐에 삽입하면 됨
   - 최초 일의 자리수를 기준으로 큐에 삽입 : 100은 0번째에, 831은 1번쨰 큐에 삽입
   - 이후 큐에서 순서대로 pop한 결과는 100, 831, 372, 344, 294, 24, 215, 15, 145, 8, 198
<div align="center">
<img src="https://github.com/user-attachments/assets/669e1aa3-2ff4-44d2-bb74-2a03e4fc1bcc">
</div>

   - 이어서 십의 자릿수를 기준으로 큐에 삽입하고 정렬 : 이 때 8과 같은 수는 008이라고 생각하여 0 위치의 큐에 삽입
<div align="center">
<img src="https://github.com/user-attachments/assets/3b366af9-70d2-400c-9bb2-671f2367a2fc">
</div>

   - 백의 자릿수 기준도 마찬가지 과정을 진행
<div align="center">
<img src="https://github.com/user-attachments/assets/6b6e6819-5f5c-464d-9dd3-d2ea7a7f4241">
</div>

   - 최종 정렬 결과 : 8, 15, 24, 100, 145, 198, 215, 294, 344, 372, 831

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/5de88c19-8ea1-465e-b3e2-88ea5da9765f">
</div>

7. 코드
```java
package sort.radixsort;

import java.io.*;

public class NumberSort3 {
    public static int[] A;
    public static long result;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());
        A = new int[N];

        for (int i = 0; i < N; i++) {
            A[i] = Integer.parseInt(br.readLine());
        }

        br.close();
        radix_sort(A, 5);

        for (int i = 0; i < N; i++) {
            bw.write(A[i] + "\n");
        }

        bw.flush();
        bw.close();
    }

    public static void radix_sort(int[] A, int max_size) {
        int[] output = new int[A.length];

        int jarisu = 1;
        int count = 0;

        while(count != max_size) { // 최대 자릿수 만큼 반복
            int[] bucket = new int[10];

            for (int i = 0; i < A.length; i++) {
                bucket[(A[i] / jarisu) % 10]++; // 일의 자리부터 시작
            }

            for (int i = 1; i < 10; i++) { // 합 배열을 이용해 index 계산
                bucket[i] += bucket[i - 1];
            }

            for(int i = A.length - 1; i >= 0; i--) { // 현재 자릿수를 기준으로 정렬
                output[bucket[(A[i] / jarisu % 10)] - 1] = A[i];
                bucket[(A[i] / jarisu) % 10]--;
            }

            for (int i = 0; i < A.length; i++) {
                // 다음 자릿수로 이동하기 위해 현재 자릿수 기준 정렬 데이터 저장
                A[i] = output[i];
            }
            jarisu *= 10; // 자릿수 증가
            count++;
        }
    }
}
```
