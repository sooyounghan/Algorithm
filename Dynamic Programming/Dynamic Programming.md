-----
### 동적 계획법
------
1. 복잡한 문제를 여러 개의 간단한 문제로 분리하여 부분의 문제를 해결함으로써 최종적으로 복잡한 문제의 답을 구하는 방법
2. 원리
   - 큰 문제를 작은 문제로 나눌 수 있어야 함
   - 작은 문제들이 반복되어 나타나고 사용되며, 이 작은 문제들의 결괏값은 항상 같아야 함
   - 모든 작은 문제들은 한 번만 계산해 DP 테이블에 저장하며, 추후 재사용할 때는 이 DP 테이블을 이용 : 이를 메모이제이션(Memoization) 기법이라고 함
   - 동적 계획법은 톱-다운(Top-Down) 방식과 바텀-업(Bottom-Up) 방식으로 구현할 수 있음

3. 예) 피보나치 수열
<div align="center">
<img src="https://github.com/user-attachments/assets/e42eaa1c-0588-4fbe-bfec-e703debc7883">
</div>

   - 동적 계획법으로 풀 수 있는지 확인하기
     + 6번쨰 피보나치 수열은 5번째 피보나치 수열과 4번쨰 피보나치 수열의 합
     + 즉, 6번째 피보나치 수열을 구하는 문제는 5번쨰 피보나치 수열과 4번쨰 피보나치 수열을 구하는 작은 문제로 나눌 수 있고, 수열의 값은 항상 같으므로 동적 계획법으로 풀 수 있음

   - 점화식 세우기
     + 점화식을 세울 때는 논리적으로 전체 문제를 나누고, 전체 문제와 부분 문제 간의 인과 관계를 파악하는 것이 중요
     + 이 예제에서는 피보나치 수열의 공식 자체가 점화식이므로 공식을 점화식으로 사용 : 피보나치 수열의 점화식은 D[i] = D[i - 1] + D[i - 2]

   - 메모이제이션 원리 이해
     + 메모이제이션이란 부분 문제를 풀었는데, 이 문제를 DP 테이블에 저장해놓고, 다음에 같은 문제가 나왔을 때 재계산 하지 않고 DP 테이블의 값을 이용하는 것을 말함
     + 아래 그림을 보면, 위에서 2번째와 3번째 피보나치 수열은 맨 왼쪽 피보나치 수열은 맨 왼쪽 탐색 부분에서 최초로 값이 구해지고, 이 떄 DP 테이블에 값이 저장
     + 이에 따라 나중에 2번째와 3번쨰 피보나치 수열의 값이 필요할 때 재연산을 이용해 구히지 않고, DP 테이블에서 바로 값을 추출
     + 이러한 방식을 사용하면 불필요한 연산과 탐색이 줄어들어 시간 복잡도 측면에서 많은 이점을 가질 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/e1dd127c-9937-44fd-8088-edeb73513190">
</div>

   - 톱-다운 구현 방식 이해
     + 위에서부터 문제를 파악해 내려오는 방식
     + 주로 재귀 함수 형태 코드를 구현
     + 코드의 가독성이 좋고, 이해하기가 편한다는 장점이 존재
```java
package dynamicprogramming.fibonacci;

import java.util.Scanner;

public class TopDown {
    public static int[] D;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        int n = sc.nextInt();
        D = new int[n + 1];
        
        for (int i = 0; i <= n; i++) {
            D[i] = -1;
        }
        
        D[0] = 0;
        D[1] = 1;
        fibo(n);

        System.out.println(D[n]);
    }

    static int fibo(int n) {
        if(D[n] != -1) { // 기존에 계산한 적이 있는 부분의 문제는 재계산하지 않고 반환
            return D[n];
        } 
        
        return D[n] = fibo(n - 2) + fibo(n - 1); 
        // 메모이제이션 : 구한 값을 바로 반환하지 않고 DP 테이블에 저장한 후 반환하도록 로직 구현
    }
}
```

   - 바텀-업 구현 방식
     + 가장 작은 부분 문제부터 문제를 해결하면서 점점 큰 문제로 확장해 나가는 방식
     + 주로 반복문 형태로 구현
```java
package dynamicprogramming.fibonacci;

import java.util.Scanner;

public class BottomUp {
    public static int[] D;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        int n = sc.nextInt();
        D = new int[n + 1];
        
        for (int i = 0; i <= n; i++) {
            D[i] = -1;
        }
        
        D[0] = 0;
        D[1] = 1;
        
        for (int i = 2; i <= n; i++) {
            D[i] = D[i - 1] + D[i - 2];
        }

        System.out.println(D[n]);
    }
}
```

   - 두 방식 중 더 안전한 방식은 바텀-업
   - 톱-다운 방식은 재귀 함수 형태로 구현되므로 재귀의 깊이가 깊어질 경우 런타임 에러 발생 가능
