-----
### 문제 39 - 최솟값을 만드는 괄호 배치 찾기 (문제 1541번)
-----
1. 문제
```
세준이는 양수와 +, - 그리고 괄호를 이용해 어떤 수식을 만들었다.
그리고 괄호를 모두 지우고, 다시 괄호를 적절히 넣어 이 수식의 값을 최소로 만들려고 한다.
이렇게 수식의 괄호를 다시 적절하게 배치해 수식의 값을 최소로 만드는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 식이 주어짐 (식은 '0' ~ '9', '+', 그리고 '-'만으로 이뤄져 있고, 가장 처음과 마지막 문자는 숫자)
   - 그리고 연속해서 2개 이상의 연산자가 나타나지 않고, 5자리를 넘게 연속되는 숫자는 없음
   - 수는 0부터 시작할 수 있으며, 입력으로 주어지는 식의 길이는 50보다 작거나 같음

3. 출력 : 1번째 줄에 정답을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/091a8bea-f28b-4a35-96b6-e0f997e65f36">
</div>

4. 1단계 : 문제 분석하기
   - 그리디 관점에서 생각하면 쉽게 풀 수 있는 문제
   - 가장 작은 최솟값을 만들기 위해서 가능한 큰 수를 빼야 함
   - 수식이 더하기와 빼기 연산만으로 이뤄져있으므로 더하기에 해당하는 부분에 괄호를 먼저 쳐서 먼저 모두 계산한 후 빼기를 실행하면 문제 해결

5. 2단계
   - 가장 먼저 더하기 연산 실행
<div align="center">
<img src="https://github.com/user-attachments/assets/81a1b268-25d2-4b36-a9db-70edba3b90df">
</div>

   - 가장 앞에 있는 값에서 더하기 연산으로 나온 결과값들을 모두 뺌
<div align="center">
<img src="https://github.com/user-attachments/assets/49adde55-5e47-4158-a06b-5728260101cc">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/595f7772-e5d8-4ea6-9b0f-66832f0d4954">
</div>

7. 코드
```java
package greedy;

import java.util.Scanner;

public class MissingParentheses {
    public static int answer = 0;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        String example = sc.nextLine();
        String[] str = example.split("-");
        
        for(int i = 0; i < str.length; i++) {
            int temp = mySum(str[i]);
            
            if(i == 0) {
                answer += temp; // 가장 앞에 있는 값만 더함
            } else {
                answer -= temp; // 뒷부분은 더한 값들을 뺌
            }
        }
        System.out.println(answer);
    }

    public static int mySum(String str) { // 나뉜 그룹의 더하기 연산 수행 함수
        int sum = 0;
        
        String[] temp = str.split("[+]");
        
        for(int i = 0; i < temp.length; i++) {
            sum += Integer.parseInt(temp[i]);
        }
        
        return sum;
    }
}
```
