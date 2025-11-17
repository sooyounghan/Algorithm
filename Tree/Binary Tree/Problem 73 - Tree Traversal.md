-----
### 문제 73 - 트리 순회하기 (문제 1991번)
-----
1. 문제
```
이진 트리를 입력받아 전위 순회(Pre-Order Traversal), 중위 순회(In-Order Traversal), 후위 순회(Post-Order Traversal)한 결과를 출력하는 프로그램을 작성하시오.
```
<div align="center">
<img src="https://github.com/user-attachments/assets/de2bdef7-097e-4c45-afc5-f91783b2b4ad">
</div>

```
예를 들어, 위와 같은 이진 트리가 입력되면 다음과 같은 결과가 나온다.
   - 전위 순회한 결과 : ABCDEFG // (루트) (왼쪽 자식) (오른쪽 자식)
   - 중위 순회한 결과 : DBAECFG // (왼쪽 자식) (루트) (오른쪽 자식)
   - 후위 순회한 결과 : DBEGFCA // (오른쪽 자식) (루트) (왼쪽 자식)
```

2. 입력
   - 1번쨰 줄에 이진 트리 노드의 개수 N(1 ≤ N ≤ 26)가 주어짐
   - 2번째 줄부터 N개의 줄에 걸쳐 각 노드와 그의 왼쪽 자식 노드, 오른쪽 자식 노드가 주어짐
     + 노드의 이름은 A부터 차례대로 영문자 대문자로 매겨짐
     + 항상 A가 루트 노드가 됨
     + 자식 노드가 없을 때는 .으로 표현

3. 출력
   - 1번째 줄에 전위 순회한 결과 출력
   - 2번째 줄에 중위 순회한 결과 출력
   - 3번째 줄에 후위 순회한 결과 출력
   - 각 줄에 N개의 알파벳을 공백 없이 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/763217f6-761b-4eb9-8c53-fb65fbdaf7e4">
</div>

4. 1단계 : 문제 분석하기
   - 문제에서 주어진 입력값을 트리 형태의 자료구조에 적절하게 저장하고, 그 안에서 탐색을 수행하는 로직 구현
   - 클래스로 노드를 구현하는 방식, 2차원 배열로 구현하는 방식 다양한 방법으로 문제 해결 가능
   - 여기서는 2차원 배열을 이용해 구현하는 방식 이용

5. 2단계
   - 2차원 배열에 트리 데이터 저장 (실제 저장할 때는 코드에서 적절하게 index화하여 저장)
<div align="center">
<img src="https://github.com/user-attachments/assets/9a3a413c-7ad3-4bfe-abcb-9f3d7c72f645">
</div>

   - 전위 순회 함수를 구현해 실행
<div align="center">
<img src="https://github.com/user-attachments/assets/0f1b5398-83b7-4bfd-aad2-c08cf74d7118">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/27725dc5-7b72-48c0-b901-3d1b7de11a8a">
</div>

   - 중위 순회, 후위 순회 함수도 2번쨰 과정과 같은 방식으로 구현해 실행
<div align="center">
<img src="https://github.com/user-attachments/assets/9a8e4bb2-5418-44c9-8303-9303a55fb239">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/70905e6c-d98a-4cd7-9cb7-2524efdbf220">
</div>

7. 코드
```java
package tree.binarytree;

import java.util.Scanner;

public class TreeTraversal {
    public static int[][] tree;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        sc.nextLine();

        tree = new int[26][2]; // 0 → 왼쪽 자식 노드(1) → 오른쪽 자식 노드 (2)

        for(int i = 0; i < n; i++) {
            String[] temp = sc.nextLine().split(" ");

            int node = temp[0].charAt(0) - 'A'; // Index로 변환하기 위해 A 문자 빼기

            char left = temp[1].charAt(0);
            char right = temp[2].charAt(0);

            // 자식 노드가 없을 때 -1을 저장
            if(left == '.') {
                tree[node][0] = -1;
            } else {
                tree[node][0] = left - 'A';
            }

            if(right == '.') {
                tree[node][1] = -1;
            } else {
                tree[node][1] = right - 'A';
            }
        }

        preOrder(0);
        System.out.println();
        inOrder(0);
        System.out.println();
        postOrder(0);
        System.out.println();
    }

    public static void preOrder(int now) {
        if(now == -1) {
            return;
        }

        System.out.print((char) (now + 'A')); // 1. 현재 노드
        preOrder(tree[now][0]); // 2. 왼쪽 탐색하기
        preOrder(tree[now][1]); // 3. 오른쪽 탐색하기
    }

    public static void inOrder(int now) {
        if(now == -1) {
            return;
        }

        inOrder(tree[now][0]); // 1. 왼쪽 탐색하기
        System.out.print((char) (now + 'A')); // 2. 현재 노드
        inOrder(tree[now][1]); // 3. 오른쪽 탐색하기
    }

    public static void postOrder(int now) {
        if(now == -1) {
            return;
        }

        postOrder(tree[now][0]); // 1. 왼쪽 탐색하기
        postOrder(tree[now][1]); // 2. 오른쪽 탐색하기
        System.out.print((char) (now + 'A')); // 3. 현재 노드
    }
}
```
