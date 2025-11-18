-----
### 문제 76 - 구간 곱 구하기 (문제 11505번)
-----
1. 문제
```
어떤 N개의 수가 주어져 있다. 그런데 중간에 수의 변경이 빈번히 일어나고, 그 사이에 어떤 구간의 곱을 구하려 한다.
만약 1, 2, 3, 4, 5라는 수가 있고, 3번째 수를 6으로 바꾸고 2번째부터 5번쨰까지 곱을 구하려고 한다면 240을 출력하면 되는 것이다.
그리고 그 상태에서 5번째 수를 2로 바꾸고 3번째부터 5번쨰까지 곱을 구하라고 한다면 48이 될 것이다.
```

2. 입력
   - 1번째 줄에 수의 개수 N(1 ≤ N ≤ 1,000,000)과 M(1 ≤ M ≤ 10,000), K(1 ≤ K ≤ 10,000)가 주어짐 (M은 수의 변경이 일어나는 횟수, K는 구간의 곱을 구하는 횟수)
   - 2번째 줄부터 N + 1번쨰 줄까지 N개의 수가 주어짐
   - N + 2번쨰 줄부터 N + M + K + 1번째 줄까지 3개의 정수 a, b, c가 주어짐
     + a가 1일 때, b번째 수를 c로 바꿈
     + a가 2일 때, b부터 c까지의 곱을 구해 출력
   - 입력으로 주어지는 모든 수는 0보다 크거나 같고, 1,000,000보다 작거나 같은 정수
  
3. 출력 : 1번째 줄부터 K줄에 걸쳐 구한 구간의 곱을 1,000,000,007으로 나눈 나머지를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/2400f792-35ba-4483-978f-c1e81cfc4e2a">
</div>

4. 1단계 : 문제 분석하기
   - 대부분 세그먼트 트리는 구간 합, 최댓값, 최솟값에 관해 많은 문제가 출제
   - 이 문제의 기본 틀은 세그먼트 트리의 다른 문제와 동일 (단, 색다른 구간 곱 문제)

5. 2단계
   - 1차원 배열로 트리의 값 초기화
     + 트리의 배열 크기가 N = 5이므로 $2^{k}$ ≥ N을 만족하는 k의 값은 3이고, 배열 크기는 $2^{3} * 2 = 16$
     + 시작 인덱스는 $2^{3}$ = start_index = 8임
     + 곱셈이므로 초깃값을 1로 저장해주고, 부모 노드를 양쪽 자식의 노드의 곱으로 표현
     + 이 때, MOD 연산을 지속적으로 수행해 값의 범위가 1,000,000,007이 넘지 않도록 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/84373c4f-0e21-4b15-969f-78666d445fd5">
</div>

   - 질의값 연산 함수와 데이터 업데이트 함수를 수행하고, 결과값을 출력 : 이 때 값을 업데이트하거나 구간 곱을 구하는 각 곱셈마다 모두 MOD 연산 수행
<div align="center">
<img src="https://github.com/user-attachments/assets/7d086096-bcdd-4942-8ab1-6ffa5e281b65">
</div>

   - 곱셈의 성질에 따라 세부 코드를 변경해야 하며, MOD 연산 로직 추가
<div align="center">
<img src="https://github.com/user-attachments/assets/0d6ae853-77c1-4daa-a15d-a2f7f92a70ff">
</div>

   - 업데이트할 때 기존 값이 0일 때
     + 기존 값이 0이었다면, 부모 노드는 모두 0으로 저장되어있는 상태이므로, 기존의 구간 합과 같이 변경된 값을 적용해도 '0 * 변경된 데이터' 형태이므로 업데이트되지 않음
     + 따라서, 이 부분은 부모 노드의 값을 업데이트 할 때, 양쪽 자식의 곱으로 업데이트하도록 세부 로직을 고민해야 함
     + 또한, 각 프로세스마다 꼼꼼하게 MOD 연산을 수행해야 함

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/ecd442dd-4a8f-4ee4-a14e-e4920b996035">
</div>

7. 코드
```java
package tree.segementtree;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class IntervalMul {
    public static long[] tree;
    public static int MOD;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken()); // 수의 개수
        int M = Integer.parseInt(st.nextToken()); // 변경이 일어나는 횟수
        int K = Integer.parseInt(st.nextToken()); // 구간 곱을 구하는 횟수

        int treeHeight = 0;
        int length = N;

        while(length != 0) {
            length /= 2;
            treeHeight++;
        }

        int treeSize = (int) Math.pow(2, treeHeight + 1);
        int leftNodeStartIndex = treeSize / 2 - 1;
        MOD = 1000000007;

        tree = new long[treeSize + 1];
        for(int i = 0; i < tree.length; i++) { // 곱셈이므로 초깃값을 1로 설정
            tree[i] = 1;
        }

        for(int i = leftNodeStartIndex + 1; i <= leftNodeStartIndex + N; i++) {
            tree[i] = Long.parseLong(br.readLine()); // 데이터를 리프 노드에 입력받기
        }

        setTree(treeSize - 1); // tree 만들기

        for (int i = 0; i < M + K; i++) {
            st = new StringTokenizer(br.readLine());

            long a = Long.parseLong(st.nextToken());
            int s = Integer.parseInt(st.nextToken());
            long e = Long.parseLong(st.nextToken());

            if (a == 1) {
                changeVal(leftNodeStartIndex + s, e);
            } else if (a == 2) {
                s = s + leftNodeStartIndex;
                e = e + leftNodeStartIndex;

                System.out.println(getMul(s, (int) e));
            } else {
                return;
            }
        }
        br.close();
    }

    private static void setTree(int i) {
        while(i != 1) {
            tree[i / 2] = tree[i / 2] * tree[i] % MOD;
            i--;
        }
    }

    private static void changeVal(int index, long val) {
        tree[index] = val;

        while(index > 1) { // 현재 노드의 양쪽 자식 노드를 찾아 곱하는 로직
            index = index / 2;
            tree[index] = tree[index * 2] % MOD * tree[index * 2 + 1] % MOD;
        }
    }

    // 모든 함수에서 곱셈이 발생할 때마다 MOD 연산 수행
    private static long getMul(int s, int e) {
        long partMul = 1;

        while(s <= e) {
            if(s % 2== 1) {
                partMul = partMul * tree[s] % MOD;
                s++;
            }

            if(e % 2 == 0) {
                partMul = partMul * tree[e] % MOD;
                e--;
            }

            s = s / 2;
            e = e / 2;
        }
        return partMul;
    }
}
```
