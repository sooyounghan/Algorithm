-----
### 문제 15 - 수 정렬하기 1 (문제 2750번)
-----
1. 문제
```
N개의 수가 주어졌을 때 이를 오름차순 정렬하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 수의 개수 N (1 ≤ N ≤ 1,000)
   - 2번쨰 줄에 N개의 줄에 숫자가 주어짐
   - 이 수는 절댓값이 1,000보다 작거나 같은 정수 (수는 중복되지 않음)

3. 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/d61c6a4b-e38a-4239-b42b-bd13bac0f012">
</div>

4. 1단계 : 문제 분석
   - 자바에서는 sort() 함수를 이용해 쉽게 정렬할 수 있음
   - N의 최대 범위가 1,000으로 매우 작으므로 O($n^{2}$) 시간 복잡도 알고리즘으로 해결 가능
   - 버블 정렬의 시간 복잡도가 O($n^{2}$)이므로 버블 정렬 알고리즘을 이용해 정렬해도 시간 복잡도 안에서 문제 해결 가능

5. 2단계
<div align="center">
<img src="https://github.com/user-attachments/assets/87030d05-c843-4aa0-84a0-d8d3c0933ec6">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/76ce57ec-2b5a-4d5b-ac0a-add4669d7825">
</div>

7. 코드
```java
package sort.bubblesort;

import java.util.Scanner;

public class BubbleSort {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int[] A = new int[N];

        for (int i = 0; i < N; i++) {
            A[i] = sc.nextInt();
        }

        for (int i = 0; i < N - 1; i++) {
            for (int j = 0; j < N - 1 - i; j++) {
                if(A[j] > A[j+1]) {
                    int temp = A[j];
                    A[j] = A[j+1];
                    A[j+1] = temp;
                }
            }
        }

        for (int i = 0; i < N; i++) {
            System.out.println(A[i]);
        }
    }
}
```
