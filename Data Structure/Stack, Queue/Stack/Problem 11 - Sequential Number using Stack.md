-----
### 스택으로 수열 만들기 (문제 1874번)
-----
1. 문제
```
1부터 n까지 수를 저장하고 출력하는 방식으로 하나의 수열을 만들 수 있다. 이 때, 스택에 push하는 순서는 반드시 오름차순을 지키도록 한다고 가정한다.
수열이 주어졌을 때 이 방식으로 주어진 수열을 만들 수 있는지 확인하고, 만들 수 있다면 push와 pop을 수행해야 하는지 확인하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 수열의 개수 n(1 ≤ n ≤ 100,000)이 주어짐
   - 2번째 줄부터 n개의 줄에는 수열을 이루는 n 이하의 자연수 1개씩 순서대로 주어짐
   - 이 때, 같은 자연수가 두 번 이상 나오지는 않음

3. 출력 : 수열을 만들기 위해 연산 순서를 출력하되, push 연산은 +, pop 연산은 -로 출력하고, 불가능할 때는 NO를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/61dcf1e8-d3d2-49c2-8bd7-7a77b10c1f5d">
</div>


4. 1단계 : 문제 분석하기
   - 이 문제는 스택의 pop, push 연산과 후입선출 개념을 이해하면 쉽게 풀 수 있음
   - 스택에 넣는 값은 오름차순 정렬이어야 함

5. 2단계
   - 이 문제는 1부터 자연수를 증가시키면서 입력으로 주어진 숫자와 비교하여 자연수를 스택에 추가하거나 빼는 방식
   - 스택 연산은 2가지 방법으로 수행
     + 현재 수열 값 ≥ 자연수
       * 현재 수열 값이 자연수보다 크거나 같을 때까지 자연수를 1씩 증가시키며 자연수를 스택에 push
       * 그리고 push가 긑나면 현재 수열을 출력하기 위해 마지막 1회만 pop
       * 예를 들어, 현재 수열 값이 4면, 스택에는 1, 2, 3, 4를 push하고 마지막 1회만 pop하여 4를 출력한 뒤, 조건문을 빠져나옴 : 자연수는 5 
     + 현재 수열 값 < 자연수
       * 현재 수열 값보다 자연수가 크다면 pop으로 스택에 있는 값을 꺼냄
       * 꺼낸 값이 현재 수열 값이거나 아닐 수 있음 : 만약 아니라면, 후입선출 원리에 따라 수열을 표현할 수 없으므로 NO를 출력한 후 문제를 종료하고, 현재 수열 값이라면 그대로 조건문을 빠져나옴
       * 앞의 예를 이어 설명하면 자연수는 5, 현재 수열 값은 3이므로 스택에서 3을 꺼냄 : 현재 수열 값과 스택에 꺼낸 값이 같으므로 계속해서 스택 연산을 수행할 수 있고, 스택에는 1, 2가 남아있으며, 자연수는 5

   - push의 한 조건문에서 수행하고, pop은 양 조건문에서 1회씩 진행
<div align="center">
<img src="https://github.com/user-attachments/assets/05b13085-aeaa-4e4f-98ac-338c2a7e8161">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/44526749-6a91-4a31-bc6d-1ca730e6f261">
</div>

7. 코드
```java
package dataStructure.stack;

import java.util.Scanner;
import java.util.Stack;

public class StackSequentialNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int[] A = new int[N];

        for(int i = 0; i < N; i++) {
            A[i] = sc.nextInt();
        }

        Stack<Integer> stack = new Stack<>();
        int num = 1;
        StringBuffer bf = new StringBuffer();
        boolean result = true;

        for (int i = 0; i < A.length; i++) {
            int su = A[i]; // 현재 수열의 수

            if(su >= num) { // 현재 수열의 값 >= 오름차순 자연수 : 값이 같아질 때까지 push() 수행
                while(su >= num) {
                    stack.push(num++);
                    bf.append("+\n");
                }
                stack.pop();
                bf.append("-\n");
            }

            else { // 현재 수열 값 < 오름차순 자연수 : pop()을 수행해 수열 원소를 꺼냄
                int n = stack.pop();

                // 스택의 가장 위의 수가 만들어야 하는 수열의 수보다 크면 수열 출력 불가
                if (n > su) {
                    System.out.println("NO");
                    result = false;
                    break;
                }

                else {
                    bf.append("-\n");
                }
            }
        }
        
        if(result) System.out.println(bf.toString());
    }
}
```
