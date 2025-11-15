-----
### 문제 55 - 거짓말 쟁이가 되긴 싫어 (문제 1043번)
-----
1. 문제
```
지민이는 파티에 갈 때마다 자기가 가장 좋아하는 이야기를 한다.
이야기는 과장할수록 재미있어지므로 되도록이면 과장해 이야기하려 한다.
문제는 몇몇 사람들이 그 이야기의 진실을 안다는 것이다.
지민이는 이야기를 과장한 게 들켜서 거짓말쟁이가 되는 건 싫어한다. 그래서 이 사람들이 파티에 왔을 때는 진실을 이야기할 수 밖에 없다.

사람의 수 N이 주어지고, 이야기의 진실을 아는 사람이 주어진다.
그리고 각 파티에 오는 사람들의 번호가 주어진다.
지민이는 모든 파티에 참가해야 한다.
이 때, 지민이가 거짓말쟁이로 알려지지 않으면서 과장된 이야기를 할 수 있는 파티 개수의 최댓값을 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 사람의 수 N과 파티의 수 M이 주어짐
   - 2번째 줄에 이야기의 진실을 아는 사람의 수와 번호가 주어짐
     + 진실을 아는 사람의 수가 먼저 주어지고, 그 개수만큼 사람들의 번호가 주어짐
     + 사람들의 번호는 1부터 N 사이의 수로 주어짐
   - 3번째 줄부터는 M개의 줄에는 각 파티마다 오는 사람의 수와 번호가 같은 방식으로 주어짐
   - N과 M은 50 이하의 자연수, 진실을 아는 사람의 수와 파티마다 오는 사람의 수는 모두 0 이상 50 이하 정수

3. 출력 : 1번쨰 줄에 문제의 정답을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/ef9db8bd-2061-4235-a905-cd7b32744929">
</div>

4. 1단계 : 문제 분석하기
   - 파티에 참석한 사람들을 1개의 집합으로 생각하고, 각각의 파티마다 union 연산을 이용해 사람들을 연결하는 것
   - 이 작업을 하면 1개의 파티에 참여한 모든 사람들은 같은 대표 노드를 바라보게 됨
   - 이후, 각 파티의 대표 노드와 진실을 알고 있는 사람들의 각 대표 노드가 동일한지 find 연산을 이용해 확인함으로써 과장된 이야기를 할 수 있는지 판단 가능

5. 2단계
   - 예제 입력 6을 이용해 해결 : 진실을 아는 사람 데이터, 파티 데이터, 유니온 파인드를 위해 대표 노드 자료구조를 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/8e0dc4fb-780a-4857-95ff-eaa4ba64713f">
</div>

   - union 연산을 수행해 각 파티에 참여한 사람들을 1개의 그룹으로 만듬
<div align="center">
<img src="https://github.com/user-attachments/assets/0673a27a-1fc6-4f8b-9991-1a08388a2a0b">
</div>

   - find 연산을 수행해 각 파티의 대표 노드와 진실을 아는 사람들이 같은 그룹에 있는지 확인 : 파티에 참여한 사람 노드는 모두 연결되어 있으므로 아무 사람이나 지정해 find 연산을 수행하면 됨
<div align="center">
<img src="https://github.com/user-attachments/assets/c424f8f2-267e-4311-a5d6-0d29b63732c0">
</div>

   - 모든 파티에 관해 3번째 과정을 반복하고, 모든 파티의 대표 노드가 진실을 아는 사람들과 다른 그룹에 있다면 결과값을 증가시킴
<div align="center">
<img src="https://github.com/user-attachments/assets/7bf611e9-c65d-4782-a440-75509a429a47">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/b3dfb3b0-adf4-40bb-b5d2-60c90a9c9882">
</div>

7. 코드
```java
package graph.unionfind;

import java.util.ArrayList;
import java.util.Scanner;

public class Liar {
    public static int[] parent;
    public static int[] trueP;
    public static ArrayList<Integer>[] party;
    public static int result;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int M = sc.nextInt();
        int T = sc.nextInt();

        result = 0;
        trueP = new int[T];

        for (int i = 0; i < T; i++) { // 진실을 아는 사람 저장
            trueP[i] = sc.nextInt();
        }

        party = new ArrayList[M];
        for (int i = 0; i < M; i++) { // 파티 데이터 저장
            party[i] = new ArrayList<>();

            int party_size = sc.nextInt();
            for (int j = 0; j < party_size; j++) {
                party[i].add(sc.nextInt());
            }
        }

        parent = new int[N + 1];
        for(int i = 0; i <= N; i++) { // 대펴 노드를 자기 자신으로 초기화
            parent[i] = i;
        }

        for(int i = 0; i < M; i++ ) { // 각 파티에 참여한 사람들을 1개의 그룹으로 만들기
            int firstPeople = party[i].get(0);

            for (int j = 1; j < party[i].size(); j++) {
                union(firstPeople, party[i].get(j));
            }
        }

        // 각 파티의 대표 노드와 진실을 아는 사람들의 대표 노드가 같다면 과장 가능
        for(int i = 0; i < M; i++) {
            boolean isPossible = true;
            int cur = party[i].get(0);

            for(int j = 0; j < trueP.length; j++) {
                if(find(cur) == find(trueP[j])) {
                    isPossible = false;
                    break;
                }
            }
            if(isPossible) {
                result++;
            }
        }

        System.out.println(result);
    }

    // union 연산
    public static void union(int a, Integer b) {
        a = find(a);
        b = find(b);
        if(a != b) {
            parent[b] = a;
        }
    }

    // find 연산
    public static int find(int a) {
        if (a == parent[a]) {
            return a;
        } else {
            return parent[a] = find(parent[a]); // 재귀 함수 형태로 구분
        }
    }
}
```
