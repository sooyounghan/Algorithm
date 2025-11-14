-----
### 문제 42 - 소수 & 팰린드롬 수 중에 최솟값 찾기 (문제 1747번)
-----
1. 문제
```
어떤 수와 그 수의 숫자 순서를 뒤집은 수가 일치하는 수를 '팰린드롬'이라 부른다.
예를 들어 79197과 324423 등이 팰린드롬 수다.

어떤 수 N (1 ≤ N ≤ 1,000,000)이 주어졌을 때 N보다 크거나 같고, 소수이면서 팰린드롬인 수 중 가장 작은 수를 구하는 프로그램을 작성하시오.
```

2. 입력 : 1번째 줄에 N이 주어짐
3. 출력 : 1번째 줄에 조건을 만족하는 수를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/e539e41a-041a-44e9-afb2-200de11d60a1">
</div>

4. 1단계 : 문제 분석하기
   - 에라토스테네스의 체를 이용해 최대 범위에 해당하는 모든 소수를 구해 놓은 후, 이 소수들의 집합에서 N보다 크거나 같으면서 팰린드롬 수인 것을 찾아내면 되는 문제
   - 단, 팰린드롬 수를 판별할 때 Integer 값의 적절한 형 변환을 이용해 좀 더 쉽게 구할 수 있는 로직이 문제 해결에 도움이 됨

5. 2단계
   - 2 ~ 10,000,000 사이에 존재하는 모든 소수를 구함 : 그 중 N보다 크거나 같은 소수에서 팰린드롬 수인지 판별
<div align="center">
<img src="https://github.com/user-attachments/assets/ac91a61a-5e7b-46c6-9f8a-d6bbb4ddc813">
</div>

   - 소수의 값을 char 배열 형태로 변환한 후, 양 끝부터 투 포인터를 비교하면 쉽게 팰린드롬 수인지 판별 가능
   - 소수 1,030,401 예시
     + char 배열로 형 변환하고, 배열과 처음과 끝을 가리키는 포인터(S, E)를 부여해 두 값을 비교
     + 두 값이 같으면 S++, E-- 연산으로 두 포인터를 이동
     + S < E를 만족할 때까지 반복해 모든 값이 같으면 팰린드롬 수로 판별
<div align="center">
<img src="https://github.com/user-attachments/assets/3728d2d2-7477-4302-a7c7-c290efd9ff7e">
</div>

   - 오름차순으로 2번째 과정을 실행하다가 최초로 팰린드롬 수가 나오면 프로그램 종료
<div align="center">
<img src="https://github.com/user-attachments/assets/615489dc-f2fc-40ff-9c8c-6169a4dc154c">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/ab42d77b-2a3e-42de-afed-495191415b46">
</div>

7. 코드
```java
package numbertheory;

import java.util.Scanner;

public class PrimeAndPalindromeNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        int[] A = new int[10000001]; // N의 범위까지 소수 구하기

        for(int i = 2; i < A.length; i++) {
            A[i] = i;
        }

        for(int i = 2; i < Math.sqrt(A.length); i++) {
            if(A[i] == 0) {
                continue;
            }

            for(int j = i + i; j < A.length; j = j + i) { // 배수 지우기
                A[j] = 0;
            }
        }

        int i = N;

        while(true) { // N부터 1씩 증가시키면서 소수와 팰린드롬 수가 맞는지 확인
            if (A[i] != 0) {
                int result = A[i];

                if (isPalindrome(result)) {
                    System.out.println(result);
                    break;
                }
            }
            i++;
        }
    }

    private static boolean isPalindrome(int target) { // 팰린드롬 수 판별 함수
        char temp[] = String.valueOf(target).toCharArray();

        int s = 0;
        int e = temp.length - 1;

        while(s < e) {
            if(temp[s] != temp[e]) {
                return false;
            }
            s++;
            e--;
        }

        return true;
    }
}
```
