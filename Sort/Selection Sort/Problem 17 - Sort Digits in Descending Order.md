-----
### 문제 17 - 내림차순으로 자릿수 정렬하기 (문제 1427번)
-----
1. 문제
```
배열을 정렬하는 것은 쉽다. 수가 주어지면, 그 수의 각 자릿수를 내림차순으로 정렬하시오.
```

2. 입력 : 1번쨰 줄에 정렬할 수 N이 주어짐 (N은 1,000,000,000보다 작거나 같은 자연수)
3. 출력 : 1번째 줄에 자릿수를 내림차순 정렬한 수 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/effa8b22-344d-47a4-b677-8c3c260a3687">
</div>

4. 1단계 : 문제 분석하기
   - 자연수를 받아 자릿수별로 정렬하는 문제 : 먼저 숫자를 각 자릿수별로 나누는 작업이 필요
   - 나머지 연산으로도 분리할 수 있지만, 여기서는 String으로 받은 후 substring() 함수를 이용해 자릿수 단위로 분리하고, 이를 다시 int형으로 변경해 배열에 저장
   - 그 다음에는 단순하게 배열을 정렬
   - 자바의 내장 함수를 사용해도 되지만, N의 길이가 크지 않으므로 선택 정렬을 상요해 내림차순 정렬 수행

5. 2단계
   - String 변수로 정렬할 데이터를 받아 int형 배열에 저장
   - 이 때 substring() 함수를 사용해 숫자를 각 자릿수별로 나눈 후 배열에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/69de22c1-ac9f-44b8-8875-e490aed2b7f2">
</div>

   - 배열의 데이터를 선택 정렬 알고리즘을 이용해 내림차순 정렬 : 내림차순 정렬이므로 최댓값을 찾아 기준이 되는 자리와 swap
<div align="center">
<img src="https://github.com/user-attachments/assets/e71b5468-b9ac-4c70-8364-7eff5a61adde">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/ec997c84-32b4-461c-9d1b-abbfbc79caf4">
</div>

7. 코드
```java
package sort.selectionsort;

import java.util.Scanner;

public class DescendingOrder {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        String str = sc.next();
        int[] A = new int[str.length()];

        for (int i = 0; i < str.length(); i++) {
            A[i] = Integer.parseInt(str.substring(i, i + 1));
        }

        for(int i = 0; i < str.length(); i++) {
            int Max = i;
            for(int j = i + 1; j < str.length(); j++) {
                if(A[j] > A[Max]) { // 내림차순이므로 최댓값을 찾음
                    Max = j;
                }
            }

            if (A[i] < A[Max]) {
                int temp = A[i];
                A[i] = A[Max];
                A[Max] = temp;
            }
        }

        for (int i = 0; i < str.length(); i++) {
            System.out.print(A[i]);
        }
    }
}
```
