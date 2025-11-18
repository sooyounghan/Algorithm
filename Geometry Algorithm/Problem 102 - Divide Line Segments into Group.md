-----
### 문제 102 - 선분을 그룹으로 나누기 (문제 2162번)
-----
1. 문제
```
N개의 선분들이 2차원 평면상에 주어져있다. 선분은 양끝점의 x, y 좌표로 표현된다.
두 선분이 서로 만나는 경우에 두 선분은 같은 그룹에 속한다고 정의하며, 그룹의 크기는 그 그룹에 속한 갯수로 정의한다.

두 선분이 만난다는 것은 선분의 끝점을 스치듯이 만나는 경우도 포함된다.
N개의 선분이 주어졌을 때, 이 선분들은 총 몇 개의 그룹으로 이루어져 있을까?
또한 가장 크기가 큰 그룹에 속한 선분의 개수는 몇 개일까?

이 2가지를 구하는 프로그램을 작성해보자.
```

2. 입력
   - 1번째 줄에 N(1 ≤ N ≤ 3,000)이 주어짐
   - 2번째 줄부터 N + 1번째 줄에 양 끝점 좌표가 $x_{1}, y_{1}, x_{2}, y_{2}$의 순서로 주어짐
   - 각 좌표의 절댓값은 5,000을 넘지 않으며, 입력되는 좌표 사이에 빈칸이 1개 이상 있음

3. 출력
   - 1번쨰 줄에 그룹의 수를 출력
   - 2번째 줄에 가장 크기가 큰 그룹에 속한 선분의 개수 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/1dd3ba32-2133-4efe-9825-d8bd62d8f386">
</div>

4. 1단계 : 문제 분석하기
   - 교차된 선분을 1개의 그룹으로 합치는 부분에서 유니온-파인드 알고리즘을 떠올릴 수 있음
   - 단, 그룹의 수와 가장 큰 그룹의 개수를 출력해야 하므로 기존 유니온-파인드 알고리즘을 적절하게 변형

5. 2단계
   - 변형된 유니온-파인드 알고리즘으로 배열 초기화 : 부모 노드 배열은 모두 -1로 초기화 (음수라면 부모 노드로 취급)
<div align="center">
<img src="https://github.com/user-attachments/assets/ed57d912-a764-4e5f-a1f7-5de48e59fc8b">
</div>

   - find 함수는 현재 인덱스에 해당하는 값이 음수라면 인덱스 값을 반환하고, 값이 양수라면 parent[i]로 이동
     + 음수라면 현재 인덱스에 해당하는 노드가 현재 선분 그룹의 대표 노드라는 뜻이 되며, 해당 음수의 절댓값이 현재 선분 그룹에 연결되어 있는 선분의 개수
   - 값을 저장하는 로직은 union 함수가 담당
     + 두 대표 노드가 같으면 반환하고, 대표 노드가 다른 경우에는 두 대표 노드를 연결해 하나의 선분 그룹으로 만듬
     + 두 대표 노드를 연결할 때는 먼저 하나로 합쳐지는 선분 그룹의 대표 노드가 될 노드(p)에 새롭게 합쳐지는 선분 그룹의 개수를 음수(q)로 더함
     + 그리고 대표 노드에서 자식 노드로 변경되는 기존 대표 노드(q)에는 최종 대표 노드(p)의 인덱스 값을 넣음
    
   - 선분을 입력받으면 이전까지 받은 선분과 교차하는지 확인
     + 교차할 때 변형된 union-find 함수를 이용해 부모 배열에 저장
     + 변형된 union-find 함수에서는 부모 배열의 값이 음수이면, 선분 그룹의 부모 노드이고, 이 음수의 절댓값이 해당 그룹의 선분 개수가 되도록 로직 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/63344826-b6e4-48e9-a112-5b320cd0c898">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/df09f2eb-17ed-4125-8742-91547ed475d8">
</div>

7. 코드
```java
package geometry;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class LineGroup {
    public static int[] parent = new int[3001];
    public static int[][] L = new int[3001][4];

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());

        for(int i = 1; i <= N; i++) {
            parent[i] = -1;
        }

        for(int i = 1; i <= N; i++) {
            st = new StringTokenizer(br.readLine());

            L[i][0] = Integer.parseInt(st.nextToken());
            L[i][1] = Integer.parseInt(st.nextToken());
            L[i][2] = Integer.parseInt(st.nextToken());
            L[i][3] = Integer.parseInt(st.nextToken());

            for(int j = 1; j < i; j++) { // 이전에 저장된 선분들과 교차 여부 확인
                if(isCross(L[i][0], L[i][1], L[i][2], L[i][3], L[j][0], L[j][1], L[j][2], L[j][3]) == true) {
                    union(i, j);
                }
            }
        }

        int ans = 0, res = 0;
        for(int i = 1; i <= N; i++) {
            if (parent[i] < 0) { // 음수이면, 선분 그룹을 대표하는 부모(대표) 노드이므로 카운트
                ans++;
            }

            res = Math.min(res, parent[i]); // 음수의 절댓값이 선분 그룹의 선분 갯수
        }

        System.out.println(ans);
        System.out.println(-res);
    }

    private static boolean isCross(long x1, long y1, long x2, long y2, long x3, long y3, long x4, long y4) {
        int abc = CCW(x1, y1, x2, y2, x3, y3);
        int abd = CCW(x1, y1, x3, y3, x4, y4);
        int cda = CCW(x3, y3, x4, y4, x1, y1);
        int cdb = CCW(x3, y3, x4, y4, x2, y2);

        if (abc * abd == 0 & cda * cdb == 0) { // 선분이 일직선 일 때
            return isOverlab(x1, y1, x2, y2, x3, y3, x4, y4);
        } else if (abc * abd <= 0 & cdb * cda <= 0) { // 선분이 교차하는 경우
            return true;
        }
        return false;
    }

    private static void union(int i, int j) {
        int p = find(i);
        int q = find(j);

        if (p == q) {
            return;
        }

        parent[p] += parent[q];
        parent[q] = p;
    }

    private static int find(int i) {
        if(parent[i] < 0) {
            return i;
        }

        return parent[i] = find(parent[i]);
    }

    static int CCW(long x1, long y1, long x2, long y2, long x3, long y3) {
        long temp = (x1 * y2 + x2 * y3 + x3 * y1) - (x2 * y1 + x3 * y2 + x1 * y3);

        if(temp > 0) {
            return 1;
        } else if (temp < 0) {
            return -1;
        }
        return 0;
    }

    private static boolean isOverlab(long x1, long y1, long x2, long y2, long x3, long y3, long x4, long y4) {
        if((Math.min(x1, x2) <= Math.max(x3, x4)) &&
                (Math.min(x3, x4) <= Math.max(x1, x2)) &&
                (Math.min(y1, y2) <= Math.max(y3, y4)) &&
                (Math.min(y3, y4) <= Math.max(y1, y2))) {
            return true;
        }

        return false;
    }
}
```
