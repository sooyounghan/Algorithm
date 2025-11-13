-----
### 문제 18 - ATM 인출 시간 계산하기 (문제 11399번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/01ed1480-3e75-473d-a8df-ba68659dfcd0">
</div>

2. 입력
   - 1번쨰 줄에 사람의 수 N (1 ≤ N ≤ 1,000)
   - 2번쨰 줄에 각 사람이 돈을 인출하는 데 걸리는 시간 $P_{i]$(1 ≤ $P_{i}$ ≤ 1,000)가 주어짐

3. 출력 : 1번쨰 줄에 각 사람이 돈을 인출하는 데 필요한 시간읯 ㅗㅇ합 중 최솟값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/ab1d7fa6-ce70-4357-80db-50bfa9bdf073">
</div>

4. 1단계 : 문제 분석
   - ATM에서 모든 사람이 가장 빠른 시간에 인출하는 방법을 그리디 방식으로 해결
   - ATM 앞에 있는 사람 중 인출 시간이 가장 적게 걸리는 사람이 먼저 인출할 수 있도록 순서를 정하는 것이 그리디 방식
   - 이렇게 하려면 인출 시간을 기준으로 정렬해야 함
   - N의 최댓값이 1,000이고, 시간 제한이 1초이므로 시간 복잡도가 O($N^{2}$) 이하인 정렬 알고리즘 중 아무거나 사용해도 되므로, 여기서는 삽입 정렬 사용
   - 정렬을 마친 후 각 사람이 돈을 인출하는 데 필요한 시간을 더함

5. 2단계
   - 삽입 정렬을 이용해 인출 시간 $P_{i}$를 기준으로 데이터를 오름차순 정렬
<div align="center">
<img src="https://github.com/user-attachments/assets/7f3e3833-4222-488d-85d9-2ee36fe28980">
</div>

   - 정렬된 데이터를 바탕으로 모든 사람이 돈을 인출하는 데 필요한 최솟값을 구함 : 인출에 필요한 시간은 앞사람들의 인출 시간 합 + 자신의 인출 시간이므로 합 배열로 해결
<div align="center">
<img src="https://github.com/user-attachments/assets/62042da0-957e-4f94-b220-af5dd161d9a2">
</div>

6. 3단계 : 슈도코드 작성하기
<div align="center">
<img src="https://github.com/user-attachments/assets/c9127e8c-7dc3-4234-940b-ca01348898ca">
</div>

7. 코드
```java
package sort.insertionsort;

import java.util.Scanner;

public class ATM {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int[] A = new int[N];
        int[] S = new int[N];

        for (int i = 0; i < N; i++) {
            A[i] = sc.nextInt();
        }

        for(int i = 0; i < N; i++){
            int insert_point = i;
            int insert_value = A[i];

            for(int j = i - 1; j >= 0; j--) { // 삽입 정렬
                if(A[j] < A[i]) {
                    insert_point = j + 1;
                    break;
                }

                if(j == 0){
                    insert_point = 0;
                }
            }

            for(int j = i; j > insert_point; j--){
                A[j] = A[j - 1];
            }

            A[insert_point] = insert_value;
        }

        S[0] = A[0]; // 합 배열 만들기

        for (int i = 1; i < N; i++) {
            S[i] = S[i - 1] + A[i];
        }

        int sum = 0; // 합 배열 총합 구하기

        for (int i = 0; i < N; i++) {
            sum += S[i];
        }

        System.out.println(sum);
    }
}
```
