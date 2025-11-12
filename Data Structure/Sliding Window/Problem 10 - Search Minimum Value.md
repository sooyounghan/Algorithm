-----
### 문제 10 - 최솟값 찾기 (문제 11003번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/88d35913-f3d3-45ae-813c-74ad6367b696">
</div>

2. 입력
   - 1번째 줄에 L(1 ≤ L ≤ N ≤ 5,000,000) 주어짐
   - 2번째 줄에 N개의 수 $A_{i}$이 주어짐 ($-10^{-9}$ ≤ $A_{i}$ ≤ $10^{9}$)

3. 출력 : 1번째 줄에 $D_{i}$를 공백으로 구분해 순서대로 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/a7b50820-e273-4c3e-9996-207e5342f7f8">
</div>

4. 1단계 : 문제 분석하기
   - 일정 범위 안에서 최솟값을 구하는 문제이므로 슬라이딩 윈도우와 정렬을 사용
   - 윈도우의 크기 : 문제에서 최솟값을 구하는 범위가 i - L + 1부터 i까지이므로, L로 생각
   - 최솟값을 위한 정렬
     + 일반적으로 정렬은 O(n log n)의 시간 복잡도를 가지므로 N과 L의 최대 범위가 5,000,000인 이 문제에서 정렬을 사용할 수 없음
     + 다시 말해, O(n)의 시간 복잡도로 해결해야 함
     + 이 때, 슬라이딩 윈도우를 덱(Deque)으로 구현하여 정렬 효과를 볼 수 있음
    
   - 덱의 구조
<div align="center">
<img src="https://github.com/user-attachments/assets/5ebbd07f-71e1-4273-88f5-1a8966be50b8">
</div>

   - 덱은 양 끝에서 데이터를 삽입하거나 삭제할 수 있는 자료구조
     + 왼쪽에서는 addFirst(), removeFirst() 함수가 삽입 / 삭제 역할
     + 오른쪽에서는 addLast(), removeLast() 함수가 삽입 / 삭제 역할을 함

5. 2단계
   - 덱에서는 (인덱스, 숫자) 형태의 노드를 클래스로 구현하여 저장
   - 덱에서 노드를 제거하는 상황을 설명하기 위해 (1, 1)과 (2, 5)를 덱에 추가할 때 필요한 탐색 / 검사 과정은 생략
<div align="center">
<img src="https://github.com/user-attachments/assets/fa520a1a-63f5-42a4-be1a-6410fab95272">
</div>

   - 이 상태에서 새 노드 (3, 2)가 덱에 저장 : 여기부터 기존 덱에 있던 노드가 제거
<div align="center">
<img src="https://github.com/user-attachments/assets/3bcfab9f-7031-4036-8c8f-88a8f8254ca0">
</div>

   - 새 노드 (3, 2)가 저장될 때 뒤에서부터 비교를 시작
     + (2, 5)는 (3, 2)보다 숫자가 크므로 (2, 5)는 덱에서 제거(removeLast)
     + 이어서 (1, 1)은 (3, 2)보다 숫자가 작으므로 탐색을 멈추고 덱에 저장(addLast)
   - 결과를 보면 (2, 5)가 덱에서 제거되어 덱에는 (1, 1), (3, 2) 순서로 노드가 오름차순 정렬 : 덱을 이용한 정렬 효과
   - 정리를 끝마친 상태의 덱을 보면 인덱스 범위는 1 ~ 3 으로 슬라이딩 윈도우 크기를 넘지 않으므로 최솟값을 찾아도 됨
   - 최솟값은 덱 처음에 있는 (1, 1) 노드

   - 계속해서 새 노드를 추가 : 인덱스 범위가 슬라이딩 윈도우를 벗어난 예
<div align="center">
<img src="https://github.com/user-attachments/assets/c0fae8a0-2547-44e6-888a-ef875be965e8">
</div>

   - 새 노드 (4, 3)은 덱 뒤에서부터 (3, 2)보다 숫자가 크므로 덱에 저장
     + 여기서 인덱스 범위에 의해 앞쪽 노드가 제거 : (1, 1), (3, 2), (4, 3)의 인덱스 범위는 1 ~ 4이므로 윈도우 범위인 3을 벗어남
     + 최솟값은 윈도우 범위 내에서 찾기로 했으므로 (1, 1)은 덱에서 제거해야 함 : 제거가 끝난 이후 최솟값을 출력하면 2

   - 다시 정리하면 숫자 비교, 윈도우 범위 계산이 끝난 덱에서 맨 앞에 있는 노드의 숫자를 출력하기만 하면 정답
   - 전체 과정
<div align="center">
<img src="https://github.com/user-attachments/assets/63dd483b-940d-41ff-8c66-423d8048dc91">
</div>

   - 최초 (1, 1)이 덱에 추가되면 비교 대상이 없고, 범위도 만족하므로 바로 1을 출력
   - (2, 5)는 (1, 1)과 숫자를 비교했을 때 더 크므로 탐색을 멈추고 덱에 추가 : 인덱스 범위가 1 ~ 2여도 윈도우 범위를 만족하므로 다시 1을 출력
   - (3, 2)는 (2, 5)와 비교했을 때 더 작으므로 (2, 5)를 덱에서 제거
     + (1, 1)은 여전히 (3, 2)보다 숫자가 작으므로 탐색을 멈추고, (3, 2)를 덱에 저장
     + 덱의 상태는 (1, 1), (3, 2)가 되고, 인덱스 범위 1 ~ 3 역시 윈도우 범위를 만족하므로 다시 1을 출력

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/e7084c9d-f473-4f8c-a68b-40286eb625ae">
</div>

7. 코드
```java
package dataStructure.slidingwindow;

import java.io.*;
import java.util.Deque;
import java.util.LinkedList;
import java.util.Scanner;
import java.util.StringTokenizer;

public class MinValueSearch {
    public static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        // 출력을 버퍼에 넣고, 한 번에 출력하기 위해 BufferedWriter 사용
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());
        int L = Integer.parseInt(st.nextToken());
        st = new StringTokenizer(br.readLine());

        Deque<Node> myDeque = new LinkedList<>();

        for(int i = 0; i < N; i++) {
            int now = Integer.parseInt(st.nextToken());

            // 새로운 값이 들어올 때마다 정렬 대신 현재 수보다 큰 값을 덱에서 제거해 시간 복잡도를 줄ㄹ임
            while(!myDeque.isEmpty() && myDeque.getLast().value > now) {
                myDeque.removeLast();
            }

            myDeque.addLast(new Node(now, i));

            // 범위에서 벗어난 값은 덱에서 제거
            if(myDeque.getFirst().index <= i - L) {
                myDeque.removeFirst();
            }

            bw.write(myDeque.getFirst().value + " ");
        }

        bw.flush();
        bw.close();
    }

    static class Node {
        public int value;
        public int index;

        public Node(int value, int index) {
            this.value = value;
            this.index = index;
        }
    }
}
```
   - 정렬 알고리즘을 사용하지 않고도 슬라이딩 윈도우와 덱을 이용해 정렬 효과를 보는 것

