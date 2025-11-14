-----
### 문제 47 - 칵테알 만들기 (문제 1033번)
-----
1. 문제
```
agust14는 세상에서 가장 맛있는 칵테일이다. 이 칵테일을 만드는 정확한 방법은 아직 세상에 공개되지 않았지만, 들어가는 재료 N개는 공개되어있다.
경근이는 인터넷 검색으로 재료 쌍 N - 1개의 비율을 알아냈고, 이 비율을 이용하면 들어가는 전체 재료의 비율을 알아낼 수 있다.

총 재료 쌍 N - 1개의 비율이 입력으로 주어질 때 다음 조건을 만족하는 칵테일을 만드는 데 필요한 각 재료의 양을 구하는 프로그램을 작성하시오.
   - 필요한 재료의 질량을 모두 더한 값이 최소가 되어야 한다
   - 칵테일을 만드는 재료의 양은 정수이고, 총 질량은 0보다 커야 한다
   - 비율은 'a b p q'와 같은 형식으로 주어지는데, a번 재료의 질량은 b번 재료의 질량으로 나눈 값이 p / q 이라는 뜻
```

2. 입력
   - 1번째 줄에 augest14를 만드는 데 필요한 재료의 개수 N이 주어지고, N은 10보다 작거나 같은 자연수
   - 2번쨰 줄부터 N - 1개의 줄에는 재료 쌍의 비율이 1줄에 1개씩 주어지는데, 문제 설명에 나온 형식 'a b p q'로 주어짐
   - 재료는 0번부터 N - 1까지이고, a와 b는 모두 N - 1보다 작거나 같은 자연수 또는 0
   - p와 q는 9보다 작거나 같은 자연수

3. 출력 : 1번째 줄에 칵테일을 만드는 데 필요한 각 재료의 질량을 0번 재료부터 순서대로 공백으로 구분해 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/26a9fdd4-0d8d-4624-84fd-7773bb9dbbc3">
</div>

4. 1단계 : 문제 분석하기
   - 문제에서 N - 1개의 비율로 N개의 재료와 관련된 전체 비율을 알아낼 수 있음
   - 이것을 그래프 관점에서 생각하면 사이클이 없는 트리 구조로 이해할 수 있음
     + 이 내용을 바탕으로 임의의 노드에서 DFS를 진행하면서 정답을 찾으면 됨
   - DFS 과정에서 유클리드 호제법을 사용해 비율들의 최소 공배수와 최대 공약수를 구하고, 재료의 최소 질량을 구하는데 사용

5. 2단계
   - 인접 리스틀 리용해 각 재료의 비율 자료를 그래프로 구현
<div align="center">
<img src="https://github.com/user-attachments/assets/ad6662de-8ba4-44d6-9e39-714df1144de8">
</div>

   - 데이터를 저장할 때마다 비율과 관련된 수들의 최소 공배수를 업데이트
<div align="center">
<img src="https://github.com/user-attachments/assets/7db51adb-46c6-4911-9e30-faffbbb48fd9">
</div>

   - 임의의 시작점에 최소 공배수 값을 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/b33b92f0-d18f-430c-869b-a3726e07b994">
</div>

   - 임의의 시작점에서 DFS로 탐색을 수행하면서 각 노드의 값을 이전 노드의 값과의 비율 계산을 통해 계산하고 저장
<div align="center">
<img src="https://github.com/user-attachments/assets/293a9419-50d2-4dd1-8345-f59525d39e62">
</div>

   - 각 노드의 값을 모든 노드의 최대 공약수로 나눈 뒤 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/e5c1ee70-7c61-489c-b529-08f1ebc80bb4">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/2ec5520e-e78b-4cb3-8fa2-721fc8af935d">
</div>

7. 코드
```java
package numbertheory.euclideanalgorithm;

import java.io.BufferedWriter;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Scanner;

public class Cocktail {
    public static ArrayList<cNode>[] A;
    public static long lcm;
    public static boolean visited[];
    public static long D[];

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = sc.nextInt();
        A = new ArrayList[N];
        visited = new boolean[N];
        D = new long[N];
        lcm = 1;

        for(int i = 0; i < N; i++) {
            A[i] = new ArrayList<cNode>();
        }

        for(int i = 0; i < N - 1; i++) {
            int a = sc.nextInt();
            int b = sc.nextInt();
            int p = sc.nextInt();
            int q = sc.nextInt();

            A[a].add(new cNode(b, p, q));
            A[b].add(new cNode(a, q, p));

            lcm *= (p * q / gcd(p, q)); // 최소 공배수는 두 수의 곱을 최대 공약수로 나눈 것
        }

        D[0] = lcm;
        DFS(0);

        long mgcd = D[0];
        for(int i = 1; i < N; i++) {
            mgcd = gcd(mgcd, D[i]);
        }

        for(int i = 0; i < N; i++) {
            System.out.print(D[i] / mgcd + " ");
        }
    }

    public static long gcd(long a, long b) { // 최대 공약수 함수 구현
        if(b == 0) {
            return a;
        } else {
            return gcd(b, a % b);
        }
    }

    public static void DFS(int Node) { // DFS 함수 구현
        visited[Node] = true;

        for (cNode i : A[Node]) {
            int next = i.getB();

            if(!visited[next]) {
                D[next] = D[Node] * i.getQ() / i.getP(); // 주어진 비율로 다음 노드 값 갱신
                DFS(next);
            }
        }
    }
}

class cNode {
    int b;
    int p;
    int q;

    public cNode(int b, int p, int q) {
        this.b = b;
        this.p = p;
        this.q = q;
    }

    public int getB() {
        return b;
    }

    public int getP() {
        return p;
    }

    public int getQ() {
        return q;
    }
}
```
