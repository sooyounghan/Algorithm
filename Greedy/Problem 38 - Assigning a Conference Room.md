-----
### 문제 38 - 회의실 배정하기 (문제 1931번)
-----
1. 문제
```
1개의 회의실에서 N개의 회의를 진행하기 위해 회의실 사용표를 만들려고 한다.
각 회의마다 시작 시간과 끝나는 시간이 주어질 때 회의 시간이 겹치지 않으면서 회의를 가장 많이 진행하려면 최대 몇 번까지 할 수 있는지 알아보자.
단, 회의를 시작하면 중간에 중단할 수 없고, 한 회의를 끝내는 것과 동시에 다음 회의를 시작할 수 있다.
회의의 시작 시간과 끝나는 시간이 같을 수 있는데, 이 때는 시작하자마자 끝나는 것으로 생각하면 된다.
```

2. 입력
   - 1번째 줄에 회의의 수 N(1 ≤ N ≤ 100,000)
   - 2번째 줄부터 N + 1줄까지 각 회의의 시작 시간과 끝나는 시간이 공백을 사이에 두고 주어짐
   - 시작 시간과 끝나는 시간은 $2^{31} - 1$보다 작거나 같은 자연수 또는 0

3. 출력 : 1번쨰 줄에 진행할 수 있는 회의의 최대 개수
<div align="center">
<img src="https://github.com/user-attachments/assets/d41b817b-2117-4d5d-9d4b-8634ce7cc3bd">
</div>

4. 1단계 : 문제 분석하기
   - 문제에서는 1개의 회의실에 회의가 겹치지 않게 최대한 많은 회의를 배정해야 함
   - 이 때, 그리디 알고리즘을 적용해야 하는데, 현재 회의의 종료 시간이 빠를 수록 다음 회의와 겹치지 않게 시작하는 데 유리
   - 그렇기 때문에, 종료 시간이 빠른 순서대로 정렬해 겹치지 않는 회의를 적절하게 선택하면 문제 해결 가능

5. 2단계
   - 회의 정보와 관련된 데이터를 저장한 후 종료 시간 순으로 나열 (단, 종료 시간이 같으면, 시작 시간을 기준으로 다시 한 번 정렬)
<div align="center">
<img src="https://github.com/user-attachments/assets/e3e59f78-19b8-4235-a13a-0193abcd1501">
</div>

   - 순차적으로 탐색하다가 시간이 겹치지 않는 회의가 나오면 선택
<div align="center">
<img src="https://github.com/user-attachments/assets/011b96b1-c29c-4661-8b22-4d7645859a2e">
</div>

   - 위의 경우 총 4개의 회의를 겹치지 않고 진행할 수 있으므로 4를 출력
   - 종료 시간이 같은 경우
     + 💡 종료 시간이 같을 때는 시작 시간이 빠른 순서대로 정렬하는 기준이 포함되어야 함
     + 문제에서 회의의 시작 시간과 종료 시간이 같을 수 있다고 했음 : 예를 들어 (2, 2), (1, 2) 2개의 회의가 있다고 가정했을 때 실제로는 2개의 회의가 겹치지 않게 할 수 있지만, 로직 상 (2, 2)가 먼저나오면 나중에 나온 (1, 2)은 불가능할 수 있음
     + 따라서 종료 시간이 같으면 시작 시간이 빠른 순서로 정렬하는 로직도 추가해야 함

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/ef31ed12-3979-443c-94b8-7f3893601828">
</div>

   - 배열의 정렬 방식은 compare() 함수를 재정의하는 방식으로 변경 가능

7. 코드
```java
package greedy;

import java.util.Arrays;
import java.util.Comparator;
import java.util.Scanner;

public class AssigningConferenceRoom {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int[][] A = new int[N][2];

        for (int i = 0; i < N; i++) {
            A[i][0] = sc.nextInt();
            A[i][1] = sc.nextInt();
        }

        Arrays.sort(A, new Comparator<int[]>() {
            @Override
            public int compare(int[] S, int[] E) {
                if(S[1] == E[1]) {
                    return S[0] - E[0];
                }
                return S[1] - E[1];
            }
        });

        int count = 0;
        int end = -1;

        for (int i = 0; i < N; i++) {
            if (A[i][0] >= end) { // 겹치지 않는 다음 회의가 나온 경우
                end = A[i][1]; // 종료 시간 업데이트
                count++;
            }
        }

        System.out.println(count);
    }
}
```
