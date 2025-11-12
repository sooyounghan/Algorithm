-----
### 문제 12 - 오큰수 구하기 (문제 17298번)
-----
1. 문제
```
크기가 N인 수열 A = A₁, A₂, ..., Aₙ이 있다. 수열의 각 원소 Aᵢ에 관련된 오큰수 NGE(i)를 구하려고 한다.
Aᵢ의 오큰수는 오른쪽에 있으면서 Aᵢ보다 큰 수 중 가장 왼쪽에 있는 수를 의미한다.
이러한 수가 없을 때 오큰수는 -1이다.
예를 들어, A = [3, 5, 2, 7]일 때, NGE(1) = 5, NGE(3) = 7, NGE(4) = -1이다.
A = [9, 5, 4, 8]일 경우에는 NGE(1) = -1, NGE(2) = 8, NGE(4) = -1
```

2. 입력
   - 1번째 줄에 수열 A의 크기 N (1 ≤ N ≤ 1,000,000)
   - 2번쨰 줄에 수열 A의 원소 $A_{1}, A_{2}, ..., A_{N}$ (1 ≤ $A_{i}$ ≤ 1,000,000)이 주어짐

3. 출력 : 총 N개의 수 NGE(1), NGE(2), ..., NGE(N)를 공백으로 구분해 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/d6b3006a-3b61-4c76-9a19-52a3379a1b53">
</div>

4. 1단계 : 문제 분석하기
   - N의 최대 크기가 1,000,000이므로 반복문으로 오큰수를 찾으면 제한 시간 초과
   - 핵심 아이디어
     + 스택에 새로 들어오는 수가 top에 존재하는 수보다 크면 그 수는 오큰수가 됨
     + 오큰수를 구한 후 수열에서 오큰수가 존재하는 숫자에 -1을 출력

5. 2단계
   - 정답 배열의 값을 모두 채운 후 출력하면 문제가 요구하는 답을 도출 가능
   - 문제 푸는 순서
     + 스택이 채워져 있고 A[index] > A[top]인 경우 pop한 인덱스를 이용해 정답 수열에 오큰수를 저장 : pop은 조건을 만족하는 동안 게속 반복
     + 위 과정을 마치면 현재 인덱스를 스택에 push하고 다음 인덱스로 넘어감
     + 위 과정을 수열 길이만큼 반복한 다음, 현재 스택에 남아 있는 인덱스에 -1을 저장
   - 예제 입력 1을 예시로 문제 푸는 순서 적용 : pop은 정답 배열에 값을 추가하는 것, push는 다음 인덱스를 보는 것
<div align="center">
<img src="https://github.com/user-attachments/assets/e8c42695-78ae-4f41-8e98-e2b2dca9ff88">
</div>

   - 처음에는 스택이 비어있으므로 과정 1 없이 과정 2를 진행
     + 인덱스 0을 push하고 다음 인덱스로 넘어감
       * A[1]은 5이고, A[top]은 3이므로, 스택에서 pop을 수행하고 Result[0]에 오큰수 5를 저장
       * 1회 반복으로 스택이 비어있으므로, pop을 더 이상 진행하지 않음
     + 인덱스 1을 push하고 다음 인덱스로 넘어감
       * A[2]은 2이고, A[top]은 5이므로 과정 2를 진행하여 push하고 다음 인덱스로 넘어감

   - 예제 입력 2에 대한 과정
<div align="center">
<img src="https://github.com/user-attachments/assets/facefd29-5930-415d-9d89-e6898576dfd5">
</div>

   - 스택이 비어 있으니 0을 push하고 다음 인덱스 1로 넘어감
     + A[0] > A[1]이므로 다시 1을 push하고, 다음 인덱스 2로 넘어감
     + A[1] > A[2]이므로 다시 2를 push하고, 다음 인덱스 3으로 넘어감
   - A[2] < A[3]이므로 pop하고 Result[2]에 8을 저장
   - 계속해서 A[1] < A[3]이므로 pop하고 Result[1]에 8 저장
   - A[0] > A[3]이므로 pop을 중단하고 3을 push
   - 다음 인덱스가 없으므로 3, 0을 순서대로 pop 하며 정답 배열에 -1을 저장

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/8271c7ec-9630-481b-8ac8-32e436f378f6">
</div>

7. 코드
```java
package dataStructure.stack;

import java.io.*;
import java.util.Stack;

public class NGE {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(br.readLine());
        int[] A = new int[n]; // 수열 배열 생성
        int[] ans = new int[n]; // 정답 배열 생성
        String[] str = br.readLine().split(" ");
        
        for (int i = 0; i < n; i++) {
            A[i] = Integer.parseInt(str[i]);
        }
        
        Stack<Integer> myStack = new Stack<>();
        myStack.push(0); // 처음에는 항상 스택이 비어있으므로 최초 값을 push 해 초기화
        
        for (int i = 0; i < n; i++) {
            // 스택이 비어 있지 않고 현재 수열이 스택 top 인덱스가 가리키는 수열보다 클 경우
            while(!myStack.isEmpty() && A[myStack.peek()] < A[i]) {
                ans[myStack.pop()] = A[i]; // 정답 배열에 오큰수를 현재 수열로 저장
            }
            myStack.push(i); // 신규 데이터 push
        }
        
        while(!myStack.isEmpty()) {
            // 반복문을 모두 순회한 뒤, 스택이 비어있지 않다면 빌 때까지 스택에 쌓인 index의 정답 배열에 -1을 넣음
            ans[myStack.pop()] = -1;
        }

        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        for (int i = 0; i < n; i++) {
            bw.write(ans[i] + " "); // 출력
        }
        
        bw.write("\n");
        bw.flush();
    }
}
```
   - 스택의 후입선출이라는 독특한 성질은 종종 시간 복잡도를 줄이거나 특정 문제 조건을 손쉽게 해결할 수 있는 실마리가 될 수 있음
