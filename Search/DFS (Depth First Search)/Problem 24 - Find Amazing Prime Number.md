-----
### 문제 24 - 신비한 소수 찾기 (문제 2023번)
-----
1. 문제
```
수빈이가 세상에서 가장 좋아하는 것은 소수이고, 취미는 소수를 이용해 노는 것이다.
요즘 수빈이가 가장 관심있어 하는 소수는 7331이다. 7331은 신기하게도 733도 소수, 73도 소수, 7도 소수다.
즉, 왼쪽부터 1자리, 2자리, 3자리, 4자릿수 모두 소수다.

수빈이는 이런 숫자를 신기한 소수라고 이름을 붙였다.
수빈이는 N의 자리 숫자 중 어떤 수들이 신기한 소수인지 궁금해졌다.
숫자 N이 주어졌을 때 N의 자리 숫자 중 신기한 소수를 모두 찾아보자.
```

2. 입력 : 1번째 줄에 N (1 ≤ N ≤ 8)이 주어짐
3. 출력 : N의 자리 숫자 중 신기한 소수를 오름차순 정렬해 1줄에 1개씩 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/2d71e807-6512-4dbb-a026-72dc8b50fa80">
</div>

4. 1단계 : 문제 분석하기
   - DFS는 재귀 함수의 형태를 보이고 있음
   - 재귀 함수를 잘 이해하면 문제 조건에 맞도록 코드를 수정하기가 쉬움

5. 2단계
   - 소수 : 약수가 1과 자기 자신뿐인 수를 의미 (예를 들어, 4는 약수가 1, 2, 4이므로 소수가 아니고, 7은 1, 7이므로 소수)
   - 💡우선 자릿수가 한 개인 소수는 2, 3, 5, 7이므로 이 수부터 탐색을 시작 (4, 6, 8, 9를 제외한 가지치기 방식 적용)
   - 이어서 자릿수가 두 개인 현재 수 * 10 + a를 계산하여 이 수가 소수인지 판단하고, 소수라면 재귀 함수로 자릿수를 하나 늘림
     + 💡 단, a가 짝수인 경우 항상 2를 약수를 가지므로 가지치기로 a가 짝수인 경우를 제외
   - 이런 방식으로 자릿수를 N까지 확장했을 때, 그 값이 소수라면 해당 값을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/c8f5deff-891c-417b-9182-6975a1f153fa">
</div>

   - 즉, DFS 형태로 탐색하며, 첫 탐색 배열, 중간 탐색 배열을 가지치기하여 시간 복잡도를 줄임
   - 또한, 중간 탐색 과정에서 소수가 아닌 경우 멈추는 가지치기도 포함되어 있음
   - 소수를 판별하는 방법은 보통 에라토스테네스의 체를 사용하지만, 여기서는 단순한 소수 판별 함수를 사용해도 가능

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/735fcc37-c586-4013-85b4-7221bcdcdf13">
</div>

7. 코드
```java
package search.dfs;

import java.util.Scanner;

public class AmazingPrimeNumber {
    static int N;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        N = sc.nextInt();

        // 일의 자리 소수는 2, 3, 5, 7이므로 4개 수에서만 시작
        DFS(2, 1);
        DFS(3, 1);
        DFS(5, 1);
        DFS(7, 1);
    }

    public static void DFS(int number, int jarisu) {
        if(jarisu == N) {
            if(isPrime(number)) {
                System.out.println(number);
            }
            return;
        }

        for(int i = 1; i < 10; i++) {
            if(i % 2 == 0) { // 짝수라면 더 이상 탐색할 필요가 없음
                continue;
            }

            if(isPrime(number * 10 + i)) { // 소수라면 재귀 함수로 자릿수를 늘림
                DFS(number * 10 + i, jarisu + 1);
            }
        }
    }

    public static boolean isPrime(int number) {
        for(int i = 2; i <= number / 2; i++) {
            if(number % i == 0) {
                return false;
            }
        }
        return true;
    }
}
```
