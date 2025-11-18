-----
### 문제 74 - 구간 합 구하기 3 (문제 2042번)
-----
1. 문제
```
어떤 N개의 수가 주어져 있다. 그런데 중간에 수의 변경이 빈번히 일어나고, 그 사이에 어떤 구간의 합을 구하려고 한다.
만약 1, 2, 3, 4, 5라는 수가 있고, 3번쨰 수를 6으로 바꾸고 2번째부터 5번째까지 합을 구하라고 한다면 17을 출력하면 되는 것이다.
그리고 이 상태에서 5번째 수를 2로 바꾸고, 3번째부터 5번쨰까지 합을 구하라고 한다면 12가 될 것이다.
```

2. 입력
   - 1번쨰 줄에 N(1 ≤ N ≤ 1,000,000)과 M(1 ≤ M ≤ 10,000), K(1 ≤ K ≤ 10,000)가 주어짐
     + N은 수의 개수, M의 수의 변경이 일어나는 횟수, K는 구간의 합을 구하는 횟수
   - 2번째 줄부터 N + 1번째 줄까지 N개의 수가 주어짐
   - 그리고 N + 2번째 줄부터 N + M + K + 1번쨰 줄까지 3개의 정수 a, b, c가 주어지는데, a가 1일 때 b(1 ≤ b ≤ N)번쨰 수를 c로 바꾸고, a가 2일 때는 b(1 ≤ b ≤ N)번째 수에서 c(b ≤ c ≤ N)번째 수까지의 합을 구해 출력
   - 입력으로 주어지는 모든 수는 $-2^{53}$보다 크거나 같고, $2^{53} - 1$보다 작거나 같은 정수

3. 출력 : 1번째 줄부터 K줄에 걸쳐 구한 구간의 합을 출력 (단, 정답은 $-2^{53}$보다 크거나 같고, $2^{53} - 1$보다 작거나 같은 정수
<div align="center">
<img src="https://github.com/user-attachments/assets/4fe93945-8693-4fbe-bf6a-956cf3e31d8b">
</div>

4. 1단계 : 문제 분석하기
   - 단순하게 구간 합을 구하는 문제라면 합 배열 자료구조를 이용해 쉽게 해결할 수 있지만, 이 문제를 합 배열로 풀지 못하는 이유는 중간에 수의 변경이 빈번하게 일어나는 상황이 존재
   - 합 배열은 자료구조 특성상 데이터 변경에 시간이 오래 걸리는 단점이 존재
   - 따라서, 이 문제는 데이터 변경에도 시간이 비교적 적게 걸리는 세그먼트 트리 자료 구조를 이용해 해결

5. 2단계
   - 1차원 배열을 이용해 트리의 값을 초기화
     + 트리 배열의 크기가 N = 5이므로 $2^{k}$ ≥ N을 만족하는 k의 값은 3이고, 배열의 크기는 $2^{3} * 2 = 16$이 됨
     + 시작 인덱스는 $2^{3}$ = start_index = 8
<div align="center">
<img src="https://github.com/user-attachments/assets/f97b23e3-b266-41e5-8cd2-6c94fbb19663">
</div>

   - 질의값 연산 함수와 데이터 업데이트 함수를 수행하고, 질의와 관련된 결과값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/0caf5357-11fe-4b48-bf3c-60e26fe4b4c3">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/beb2f567-da14-4158-873e-646c2e37db2f">
</div>

7. 코드
```java
package tree.segementtree;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class SumInterval {
    public static long[] tree;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken()); // 수의 개수
        int M = Integer.parseInt(st.nextToken()); // 변경이 일어나는 횟수
        int K = Integer.parseInt(st.nextToken()); // 구간 합을 구하는 함수

        int treeHeight = 0;
        int length = N;

        while(length != 0) {
            length /= 2;
            treeHeight++;
        }

        int treeSize = (int) Math.pow(2, treeHeight + 1);
        int leftNodeStartIndex = treeSize / 2 - 1;

        tree = new long[treeSize + 1];

        // 데이터를 리프 노드에 입력받기
        for(int i = leftNodeStartIndex + 1; i <= leftNodeStartIndex + N; i++) {
            tree[i] = Long.parseLong(br.readLine());
        }

        setTree(treeSize - 1); // 트리 만들기

        for(int i = 0; i < M + K; i++) {
            st = new StringTokenizer(br.readLine());

            long a = Long.parseLong(st.nextToken());
            int s = Integer.parseInt(st.nextToken());
            long e = Long.parseLong(st.nextToken());

            if(a == 1) { // 변경하기
                changeVal(leftNodeStartIndex + s, e);
            } else if (a == 2) { // 구간 합
                s = s + leftNodeStartIndex;
                e = e + leftNodeStartIndex;

                System.out.println(getSum(s, (int) e));
            } else {
                return;
            }
        }
        br.close();
    }

    private static void setTree(int i) { // 초기 트리 구성 함수
        while(i != 1) {
            tree[i / 2] += tree[i];
            i--;
        }
    }

    private static void changeVal(int index, long val) { // 값을 변경하는 함수
        long diff = val - tree[index];

        while(index > 0) {
            tree[index] = tree[index] + diff;
            index = index / 2;
        }
    }

    private static long getSum(int s, int e) { // 구간 합을 구하는 함수
        long partSum = 0;

        while(s <= e) {
            if(s % 2 == 1) {
                partSum += tree[s];
                s++;
            }

            if(e % 2 == 0) {
                partSum += tree[e];
                e--;
            }

            s = s / 2;
            e = e / 2;
        }

        return partSum;
    }
}
```
