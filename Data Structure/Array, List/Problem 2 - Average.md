-----
### 문제 2 - 평균 구하기 (1546번)
-----
1. 문제
```
세준이는 기말고사를 망쳤다. 그래서 점수를 조작해 집에 가져가기로 결심했다. 일단 세준이는 자기 점수 중 최댓값을 골랐다.
그런 다음 최댓값을 M이라 할 때, 모든 점수를 점수 / M * 100으로 고쳤다. 예를 들어, 세준이의 최고점이 70점, 수학 점수 50점이라면
수학 점수는 50 / 70 * 100이므로 71.43점이다. 세준이의 성적을 이 방법을 계산했을 때, 새로운 평균을 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 시험을 본 과목의 개수 N이 주어지며, 해당 값은 1,000보다 작거나 같음
   - 2번쨰 줄에 세준이의 현재 성적이 주어짐
   - 해당 값은 100보다 작거나 같은, 음이 아닌 정수이고, 적어도 1개의 값은 0보다 큼

3. 출력 : 1번쨰 줄에 새로운 평균을 출력하며, 실제 정답과 출력값의 절대 오차 또는 상대 오차가 $10^{-2}$ 이하이면 정답
<div align="center">
<img src="https://github.com/user-attachments/assets/1883947a-63a8-434e-8485-3160b614e9ee">
</div>

4. 1단계 : 문제 분석하기
   - 최고 점수를 기준으로 전체 점수를 다시 계산해야 하므로 모든 점수를 입력받은 후 최고점을 별도로 저장해야 함
   - 또한 문제에서 제시한 한 과목의 점수를 계산하는 식은 총합과 관련된 식으로 변환할 수 있음
   - 따라서, 일일히 변환 점수를 구할 필요 없이 한 벙네 변환한 점수의 평균 점수를 구할 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/b838be7e-7c80-459c-9327-44394b2d693e">
</div>

5. 2단계
   - 점수를 1차원 배열에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/88786ddd-5000-4bd5-8972-40aaa0f19889">
</div>

   - 배열을 탐색하며, 최고 점수와 점수의 총합을 구함
<div align="center">
<img src="https://github.com/user-attachments/assets/7252a825-2c46-4df8-8ed2-aff678a84a42">
</div>

   - '총합 * 100 / 최고 점수 / 과목의 수'를 계산해 다시 계산한 점수의 평균값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/e8130505-a76a-4c50-a2e6-94f8e9d319c8">
</div>

6. 3단계 : 슈도코드 작성하기
<div align="center">
<img src="https://github.com/user-attachments/assets/b4b9eceb-cd73-4ac1-81b2-d01772da5dc7">
</div>

7. 코드 구현
```java
package dataStructure.array;

import java.util.Scanner;

public class Average {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int A[] = new int[N];

        for (int i = 0; i < N; i++) {
            A[i] = sc.nextInt();
        }

        long sum = 0;
        long max = 0;

        for (int i = 0; i < N; i++) {
            if (A[i] > max) {
                max = A[i];
            }

            sum += A[i];
        }

        // 한 과목과 관련된 수식을 총합과 관련된 수식으로 변환
        System.out.println(sum * 100.0 / max / N);
    }
}
```
