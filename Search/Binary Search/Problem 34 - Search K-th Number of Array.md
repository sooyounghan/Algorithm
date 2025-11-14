-----
### 문제 34 - 배열에서 K번쨰 수 찾기 (문제 1300번)
-----
1. 문제
```
세준이는 크기가 N X N인 배열 A를 만들었다. 배열에 들어 있는 수는 A[i][j] = i X j이다.
이 수를 1차원 배열 B에 넣으면 크기는 N X N이 된다.
B를 오름차순으로 정렬했을 때 B[k]를 구하라 (배열 A와 B의 인덱스는 1부터 시작한다.)
```

2. 입력
   - 1번째 줄에 배열의 크기 N이 주어지며, N은 $10^{5}$보다 작거나 같은 자연수
   - 2번째 줄에 k가 주어지며, k는 min($10^{9}$, $N^{2}$)보다 작거나 같은 자연수

3. 출력 : B[k]를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/b74d88dc-c734-4add-914b-5a7a3b195066">
</div>

4. 1단계 : 문제 분석하기
   - k의 범위가 1 ~ min($10^{9}$, $N^{2}$)이므로 시간 복잡도가 $N^{2}$인 알고리즘은 사용 불가
   - 여기서는 이진 탐색을 사용 : 이진 탐색으로 중앙값보다 작은 수의 개수를 세면서 범위를 절반씩 줄이는 방법으로 B[k]를 구함
   - 다시 말해 작은 수의 개수가 k - 1개인 중앙값이 정답 : 작은 수의 개수를 세는 아이디어가 이 문제를 푸는 열쇠

5. 2단계
   - 2차원 배열은 N행이 N의 배수로 구성되어 있으므로 2차원 배열에서 k번째 수는 k를 넘지 않음
   - 다시 말해, 2차원 배열의 1 ~ k번쨰 안에 정답이 존재 : 이진 탐색의 시작 인덱스를 1, 종료 인덱스를 k로 설정
   - N = 3, k = 7일 때의 예
<div align="center">
<img src="https://github.com/user-attachments/assets/e2cff468-779e-4803-afa3-478a4e4ed68f">
</div>

   - 최초의 중앙값은 4 (각 행에서 중앙값보다 작거나 같은 수의 개수는 중앙값을 각 행의 첫 번쨰 값으로 나눔)
   - 단, 나눈 값이 N보다 크면 N으로 정함
     + 1열은 1로 나눠 4, 2열은 2로 나눠 2, 3열은 3으로 나눠 1을 얻음
     + 그 결과 각 열에서 중앙값 4보다 작거나 같은 수의 개수는 3, 2, 1로 6개가 됨
   - 중앙값보다 작거나 같은 수의 개수는 Math.min(middle / i, N)으로 계산
   - 그리고 이를 통해 중앙값 4는 6번째 수보다 큰 수가 될 수 없다는 것을 알 수 있으며, 중앙값 4보다 큰 범위에 정답이 있다는 것을 유추 가능

   - 정리
     + 중앙값보다 작은 수의 개수가 k보다 작으면 시작 인덱스 = 중앙값 + 1
     + 중앙값보다 작은 수의 개수가 k보다 크거나 같으면 종료 인덱스 = 중앙값 - 1, 정답 변수 = 중앙값

   - N = 3, k = 7을 찾는 과정
<div align="center">
<img src="https://github.com/user-attachments/assets/98090be9-9588-4c19-bca7-fe0b2a747d9b">
</div>

   - 1번째 중앙값은 4이며 4보다 작거나 같은 수의 개수는 6(k보다 작음)이므로 시작 인덱스의 중앙값 + 1 = 5, 종료 인덱스를 7로 하고 정답 업데이트는 하지 않음
   - 계속해서 이진 탐색을 진행 : 2번쨰 중앙값은 6이며, 6보다 작거나 같은 수의 개수는 8(k보다 크거나 같음)이므로 시작 인덱스는 5, 종료 인덱스는 중앙값 - 1 = 5로 하고 정답을 6으로 업데이트
   - 아직 시작 인덱스가 종료 인덱스보다 크지 않으므로 계속해서 이진 탐색 진행 : 3번째 중앙값은 5이며 5보다 작거나 같은 수의 개수는 6(k보다 작음)이므로 시작 인덱스를 중앙값 + 1 = 6, 종료 인덱스를 5로 설정
   - 시작 인덱스가 종료 인덱스보다 크므로 이진 탐색을 종료
   - 이 과정에서 업데이트한 정답은 6이므로 정답은 6

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/8e299de1-6bc3-4c8c-9531-5e7e49401013">
</div>

7. 코드
```java
package search.binarysearch;

import java.util.Scanner;

public class KthNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int K = sc.nextInt();
        long start = 1, end = K;
        long ans = 0;

        // 이진 탐색 수행하기
        while(start <= end) {
            long mid = (start + end) / 2;
            long cnt = 0;

            // 중앙값보다 작은 수는 몇 개인지 계산
            for(int i = 1; i <= N; i++) {
                cnt += Math.min(mid / i, N); // 작은 수를 카운트 하는 핵심 로직
            }

            if(cnt < K) {
                start = mid + 1;
            } else {
                ans = mid; // 현재 단계의 중앙값을 정답 변수에 저장
                end = mid - 1;
            }
        }
        System.out.println(ans);
    }
}
```
