-----
### 문제 75 - 최솟값 찾기 2 (문제 10868번)
-----
1. 문제
```
N (1 ≤ N ≤ 100,000)개의 정수들이 있을 때, A번째에서 B번째까지 나열된 정수 중 가장 작은 정수를 찾는 것은 어려운 일이 아니다.
하지만 이와 같은 a, b의 쌍이 M(1 ≤ M ≤ 100,000)개 주어졌을 때는 어려운 문제가 된다. 이 문제를 해결해보자.

여기서 A번째라는 것은 입력되는 순서로 A번째라는 이야기다.
예를 들어, a = 1, b = 3이라면 입력된 순서대로 1번, 2번, 3번 정수 중 최솟값을 찾아야 한다.
각각의 정수들은 1 이상 1,000,000,000 이하의 값을 갖는다.
```

2. 입력
   - 1번째 줄에 N, M이 주어짐
   - 다음 N개의 줄에는 N개의 정수가 주어짐
   - 다음 M개의 줄에는 a, b의 쌍이 주어짐

3. 출력 : M개의 줄에 입력받은 순서대로 각 a, b와 관련된 답을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/683b51a7-c4c8-427b-a766-431f2924afef">
</div>

4. 1단계 : 문제 분석하기
   - 전형적인 세그먼트 트리 문제
   - 데이터를 변경하는 부분이 없으므로 1차원 배열에 최솟값 기준으로 트리 데이터를 저장하고, 질의를 수행하는 함수까지만 구현

5. 2단계
   - 1차원 배열로 트리의 값을 최솟값 기준으로 초기화
     + 트리 배열 크기가 N = 10이므로 $2^{K}$ ≥ N을 만족하는 k의 값은 4이고, 배열의 크기는 $2^{4} * 2 = 32$가 됨
     + 시작 인덱스는 $2^{4}$ = start_index = 16임
<div align="center">
<img src="https://github.com/user-attachments/assets/c778b097-d824-4039-a1a6-b9f5c8632c8e">
</div>

   - 질의값 연산 함수를 수행하고, 결과값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/8dfc8d2a-a644-4036-9f77-49cd7a1e7393">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/3332fe3e-3fdb-420b-b85a-65aae0ace31">
</div>

7. 코드
```java
package tree.segementtree;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Minimum {
    public static long[] tree;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken()); // 수의 개수
        int M = Integer.parseInt(st.nextToken()); // 구간의 최솟값을 구하는 횟수

        int treeHeight = 0;
        int length = N;

        while(length != 0) {
            length /= 2;
            treeHeight++;
        }

        int treeSize = (int) Math.pow(2, treeHeight + 1);
        int leftNodeStartIndex = treeSize / 2 - 1;

        // 트리 초기화 하기
        tree = new long[treeSize + 1];

        for(int i = 0; i < tree.length; i++) {
            tree[i] = Integer.MAX_VALUE;
        }

        // 데이터 입력받기
        for(int i = leftNodeStartIndex + 1; i <= leftNodeStartIndex + N; i++) {
            tree[i] = Long.parseLong(br.readLine());
        }

        setTree(treeSize - 1); // 트리 만들기

        for(int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());

            int s = Integer.parseInt(st.nextToken());
            int e = Integer.parseInt(st.nextToken());

            s = s + leftNodeStartIndex;
            e = e + leftNodeStartIndex;

            System.out.println(getMin(s, e));
        }

        br.close();
    }

    private static void setTree(int i) { // 초기 트리를 구성하는 함수
        while(i != 1) {
            if(tree[i / 2] > tree[i]) {
                tree[i / 2] = tree[i];
            }
            i--;
        }
    }

    private static long getMin(int s, int e) { // 범위의 최솟값을 구하는 함수
        long Min = Integer.MAX_VALUE;

        while (s <= e) {
            if (s % 2 == 1) {
                Min = Math.min(Min, tree[s]);
                s++;
            }

            s = s / 2;

            if (e % 2 == 0) {
                Min = Math.min(Min, tree[e]);
                e--;
            }

            e = e / 2;
        }

        return Min;
    }
}
```
