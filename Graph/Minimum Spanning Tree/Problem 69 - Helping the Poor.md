-----
### 문제 69 - 불우이웃돕기 (문제 1414번)
-----
1. 문제
```
다솜이는 불우이웃돕기 활동을 하기 위해 무엇을 해야 할 것인지 생각했다.
마침 집에 엄청나게 많은 랜선이 있다는 것을 깨달았다.
랜선이 이렇게 많이 있을 필요가 없다고 느낀 다솜이는 랜선을 이용해 지역 사회에 봉사하기로 했다.

다솜이의 집에는 N개의 방이 있다.
각각의 방에는 모두 1개의 컴퓨터가 있다. 각각의 컴퓨터는 랜선으로 연결되어 있다.
어떤 컴퓨터 A와 B가 있을 때, A와 B가 서로 랜선으로 연결되어 있거나 다른 컴퓨터를 이용해 연결되어 있으면 서로 통신할 수 있다.

다솜이는 집 안에 있는 N개의 컴퓨터를 모두 서로 연결되게 하고 싶다.
N개의 컴퓨터가 서로 연결되어 있는 랜선의 길이가 주어질 때, 다솜이가 기부할 수 있는 랜선 길이의 최대값을 출력하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 컴퓨터의 개수 N이 주어짐
   - 2번째 줄부터 랜선의 길이가 주어짐
     + i번째 줄의 j번째 문자가 0일 경우, 컴퓨터 i와 j를 연결하는 랜선이 없다는 것을 의미
     + 이 외에의 경우에는 랜선의 경우를 의미
   - 랜선의 길이 a에서 z는 1부터 26, A에서 Z는 27부터 52를 나타냄
   - N은 100보다 작거나 같은 자연수

3. 출력 : 1번쨰 줄에 다솜이가 기부할 수 있는 랜선 길이의 최댓값을 출력 (만약, 모든 컴퓨터가 연결되어 있지 않다면 -1을 출력)
<div align="center">
<img src="https://github.com/user-attachments/assets/ba493921-8cba-4ded-9f51-17e2f461b258">
</div>

4. 1단계 : 문제 분석하기
   - 인접 행렬의 형태로 데이터가 들어오므로 이 부분을 최소 신장 트리가 가능한 형태로 변형하는 것이 핵심
   - 먼저 문자열로 주어진 랜선의 길이를 숫자로 변형해 랜선의 총합을 저장
   - 이 때, i와 j가 같은 곳의 값은 같은 컴퓨터를 연결한다는 의미로 별도 엣지에 저장하지 않고, 나머지 위치의 값들을 i → j로 가는 엣지로 생성하고, 엣지 리스트에 저장하면 최소 신장 트리 문제로 변형할 수 있음

5. 2단계
   - 입력 데이터 정보를 배열에 저장 : 먼저 입력으로 주어진 문자열을 숫자로 변환해 총합으로 저장
     + 소문자는 '현재 문자 - 'a' + 1', 대문자는 '현재 문자 - 'A' + 27'로 변환
     + i와 j가 다른 경우에는 다른 컴퓨터를 연결하는 랜선이므로 엣지 리스트에 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/55f0cd28-e567-45a5-867f-3fa7554f84a3">
</div>

   - 저장된 엣지 리스트를 바탕으로 최소 신장 트리 알고리즘을 수행
<div align="center">
<img src="https://github.com/user-attachments/assets/1f4c817e-9367-477b-8190-fe06c6518faa">
</div>

   - 최소 신장 트리의 결과값이 다솜이가 최소한으로 필요한 랜선 길이이므로 처음 저장해 둔 래넌의 총합에서 최소 신장 트리의 결과값을 뺀 값을 정답으로 출력
     + 단, 최소 신장 트리에서 사용한 엣지 개수가 N - 1가 아닌 경우, 모든 컴퓨터를 연결할 수 없다는 뜻으로 -1을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/93bf7dc3-9e80-4b7c-b7aa-9d6cee43ec3e">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/f53bc89e-1dc6-4f22-b218-9483f6881d61">
</div>

7. 코드
```java
package graph.mst;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class HelpPoor {
    public static int[] parent;
    public static int N, sum;
    public static PriorityQueue<lEdge> queue;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());

        queue = new PriorityQueue<lEdge>();

        for(int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());

            char[] tempC = st.nextToken().toCharArray();

            for (int j = 0; j < N; j++) {
                int temp = 0;

                if (tempC[j] >= 'a' && tempC[j] <= 'z') {
                    temp = tempC[j] - 'a' + 1;
                } else if (tempC[j] >= 'A' && tempC[j] <= 'Z') {
                    temp = tempC[j] - 'A' + 27;
                }

                sum = sum + temp;

                if (i != j && temp != 0) {
                    queue.add(new lEdge(i, j, temp));
                }
            }
        }

        parent = new int[N];

        for(int i = 0; i < parent.length; i++) {
            parent[i] = i;
        }

        int useEdge = 0;
        int result = 0;

        // 최소 신장 트리 알고리즘 수행하기
        while(!queue.isEmpty()) {
            lEdge now = queue.poll();

            if(find(now.s) != find(now.e)) { // 같은 부모가 아니라면 연결
                union(now.s, now.e);
                result = result + now.v;
                useEdge++;
            }
        }

        if(useEdge == N - 1) {
            System.out.println(sum - result);
        } else {
            System.out.println(-1);
        }
    }

    public static int find(int a) { // find 연산
        if(a == parent[a]) {
            return a;
        } else {
            return parent[a] = find(parent[a]); // 재귀 함수 형태로 구현 : 경로 압축
        }
    }

    public static void union(int a, int b) {
        a = find(a);
        b = find(b);

        if (a != b) {
            parent[b] = a;
        }
    }
}

class lEdge implements Comparable<lEdge> {
    int s, e, v;

    public lEdge(int s, int e, int v) {
        this.s = s;
        this.e = e;
        this.v = v;
    }

    @Override
    public int compareTo(lEdge o) {
        return this.v - o.v;
    }
}
```
