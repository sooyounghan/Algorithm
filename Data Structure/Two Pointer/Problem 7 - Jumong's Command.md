-----
### 문제 7 - 주몽의 명령
-----
1. 문제
```
주몽은 철기군을 양성하기 위한 프로젝트에 나섰다. 그래서 야철대장에게 철기군이 입을 갑옷을 만들라고 명령했다.
야철대장은 주몽의 명령에 따르기 위해 연구에 착수하더 중 갑옷을 만드는 재료들은 각각 고유한 번호가 있고,
갑옷은 2개의 재료를 만드는 데 2가지 재료의 고유한 번호를 합쳐 M(1 ≤ M ≤ 10,000,000)이 되면 갑옷이 만들어지는 사실을 발견했다.
야철대장은 자신이 가진 재료로 갑옷을 몇 개나 만들 수 있는지 궁금해졌다.
야철대장의 궁금증을 풀어주기 위해 N(1 ≤ N ≤ 15,000)개의 재료와 M이 주어졌을 때 몇 개의 갑옷을 만들 수 있는지 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 재료의 개수 N(1 ≤ N ≤ 15,000), 2번째 줄에 갑옷을 만드는데 필요한 수 M(1 ≤ M ≤ 10,000,000)이 주어짐
   - 3번쨰 줄에는 N개의 재료들이 가진 고유한 번호들이 공백을 사이에 두고 주어짐
   - 고유한 번호는 100,000보다 작거나 같은 자연수

3. 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/84f44326-4126-4ef6-8942-4c21411d5e51">
</div>

4. 1단계 : 문제 분석하기
   - 시간 복잡도 : 두 재료의 번호의 합, 즉 크기를 비교하므로 값을 정렬하면 문제를 좀 더 쉽게 풀 수 있음
   - N의 최대 범위가 15,000이므로 O(n log n) 알고리즘을 사용해도 문제 없음 : 일반적으로 정렬 알고리즘의 시간 복잡도는 O(n log n)
   - 즉, 정렬을 사용해도 괜찮으며, 입력받은 N개의 재룟값을 정렬한 다음 양쪽 끝위 위치로 투 포인터로 지정해 문제 접근

5. 2단계
   - 재료 데이터를 배열 A[N]에 저장한 후 오름차순 정렬
<div align="center">
<img src="https://github.com/user-attachments/assets/0bd5efcf-3421-4839-a7ed-649f79e61a54">
</div>

   - 투 포인터 i, j를 양쪽 끝에 위치시킨 후 문제 조건에 적합한 포인터 이동 원칙을 이용해 탐색 수행
     + A[i] + A[j] > M : j--; (번호의 합이 M보다 크므로 큰 번호 index를 내림)
     + A[i] + A[j] < M : i++; (번호의 합이 M보다 작으므로 작은 번호 index를 올림)
     + A[i] + A[j] == M :i++; j--; count++; (양쪽 포인터를 이동시키고 count 증가)

   - i와 j가 만날 때까지 반복 : 반복이 끝나면 count 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/0e424755-039d-4978-b81f-88c69ae7888c">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/32ade613-ad1a-4879-a962-31c1091e6e3e">
</div>

7. 코드
```java
package dataStructure.twopointer;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class JumongCommand {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());
        int M = Integer.parseInt(br.readLine());

        int[] A = new int[N];

        StringTokenizer st = new StringTokenizer(br.readLine());

        for(int i = 0; i < N; i++) {
            A[i] = Integer.parseInt(st.nextToken());
        }

        Arrays.sort(A);

        int count = 0;
        int i = 0;
        int j = N - 1;

        while(i < j) { // 두 포인터 이동 원칙에 따라 포인터 이동하여 처리
            if(A[i] + A[j] < M) {
                i++;
            } else if(A[i] + A[j] > M) {
                j--;
            } else{
                count++;
                i++;
                j--;
            }
        }

        System.out.println(count);
        br.close();
    }
}
```
