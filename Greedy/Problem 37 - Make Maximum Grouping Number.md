-----
### 문제 37 - 수를 묶어서 최댓값 만들기 (문제 1744번)
-----
1. 문제
```
길이가 N인 수열이 주어질 때 수열의 합을 구하려고 한다. 그런데 수열의 합을 구하기 전 먼저 수열 안에 있는 임의의 두 수를 묶으려 한다.
위치에 상관없이 두 수를 묶을 수 있다. 단, 같은 위치에 있는 수(자기 자신)를 묶을 수는 없다. 묶인 두 수는 수열의 합을 구할 때 서로 곱한 후 계산한다.
수열의 모든 수는 각각 한 번씩만 묶을 수 있다.

예를 들어, 어떤 수열이 {0, 1, 2, 4, 3, 5}일 때, 그냥 이 수열의 합만 구하면 1 + 2 + 3 + 4 + 5 = 15이다.
하지만, 2와 3을 묶고 4와 5를 묶으면 0 + 1 + (2 * 3) + (4 * 5) = 27이 되어 최댓값이 나온다.

주어진 수열의 각 수를 적절히 묶어 그 합을 최대로 만드는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 수열의 크기 N이 주어짐 (N은 10,000보다 작은 자연수)
   - 2번째 줄부터 N개의 줄에 수열의 각 수가 주어짐 (수열의 수는 -10,000보다 크거나 같고, 10,000보다 작거나 같은 정수)

3. 출력 : 합이 최대가 나오게 수를 묶었을 때 그 합을 출력 (정답은 항상 $2^{31}$보다 작음)
<div align="center">
<img src="https://github.com/user-attachments/assets/f4b071ea-1171-47b3-a1af-bb8e90cd0ea3">
</div>

4. 1단계 : 문제 분석하기
   - N의 최대 범위가 10,000이므로 시간 복잡도와 관련된 제약은 적은 문제
   - 가능한 한 큰수들끼리 묶어야 결과값이 커진다는 것을 알 수 있음 : 주어진 수열이 1, 2, 3, 4라면 1 * 4 + 2 * 3보다 1 * 2 + 3 * 4의 결과값이 더 큼
   - 또한, 음수끼리 곱하면 양수로 변하는 성질을 추가로 고려

5. 2단계
   - 수의 집합을 1보다 큰 수, 1 , 0, 음수 4가지 유형으로 나누어 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/bdb4334e-4e4b-4981-ab76-ce81ef7a7341">
</div>

   - 1보다 큰 수의 집합을 정렬해 최댓값부터 차례대로 곱한 후 더함 : 원소의 개수가 홀수일 때 마지막 남은 수는 그대로 더함
<div align="center">
<img src="https://github.com/user-attachments/assets/f2ee83d1-0a68-4f44-b413-15c067698ee6">
</div>

   - 음수의 집합을 정렬해 최솟값부터 차례대로 곱한 후 더함 : 원소의 개수가 홀수일 때 수열에 0이 있다면 1개 남는 음수를 0과 곱해 0을 만들고, 수열에 0이 없다면 그대로 더함
<div align="center">
<img src="https://github.com/user-attachments/assets/ed9533d3-143d-4276-ba43-3f3362752253">
</div>

   - 2 ~ 3번째 과정에서 구한 값을 더하고, 그 값에 숫자 1의 개수를 더함
<div align="center">
<img src="https://github.com/user-attachments/assets/e3d4fb66-c93e-4da6-a97d-cae0ece91d4e">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/9a328755-c623-449e-9abc-352db80ecca0">
</div>

7. 코드
```java
package greedy;

import java.util.Collections;
import java.util.PriorityQueue;
import java.util.Scanner;

public class GroupingNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();

        // 양수는 내림차순 정렬하기
        PriorityQueue<Integer> plusPq = new PriorityQueue<>(Collections.reverseOrder());
        PriorityQueue<Integer> minusPq = new PriorityQueue<>();

        int one = 0;
        int zero = 0;

        for (int i = 0; i < N; i++) { // 4개의 그룹을 분리해 저장
            int data = sc.nextInt();

            if(data > 1) {
                plusPq.add(data);
            } else if (data == 1) {
                one++;
            } else if (data == 0) {
                zero++;
            } else {
                minusPq.add(data);
            }
        }

        int sum = 0;

        // 양수 처리하기
        while(plusPq.size() > 1) {
            int first = plusPq.remove();
            int second = plusPq.remove();
            sum += (first * second);
        }

        if (!plusPq.isEmpty()) {
            sum += plusPq.remove();
        }

        // 음수 처리하기
        while (minusPq.size() > 1) {
            int first = minusPq.remove();
            int second = minusPq.remove();
            sum += (first * second);
        }

        if(!minusPq.isEmpty()) {
            if(zero == 0) {
                sum += minusPq.remove();
            }
        }

        // 1 처리하기
        sum += one;
        System.out.println(sum);
    }
}
```

