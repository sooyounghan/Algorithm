-----
### 디버깅 활용 사례 살펴보기
-----
1. 합 관련 코드
```java
package debugging;

import java.util.Scanner;

public class debuggingError {
    public static void main(String[] args) {
        // 배열에서 주어진 범위의 합을 구하는 프로그램

        Scanner sc = new Scanner(System.in);

        int testCase = sc.nextInt();
        int answer = 0;

        int A[] = new int[100001];
        int S[] = new int[100001];

        for (int i = 1; i < 10000; i++) {
            A[i] = (int) (Math.random() * Integer.MAX_VALUE);
            S[i] = S[i - 1] + A[i];
        }

        for (int i = 0; i < testCase; i++) {
            int query = sc.nextInt();

            for (int j = 0; j < query; j++) {
                int start = sc.nextInt();
                int end = sc.nextInt();

                answer += S[end] - S[start - 1];
                System.out.println(testCase + " " + answer);
            }
        }
    }
}
```

2. 변수 초기화 오류
   - 변수 초기화 로직에서 초기화를 제대로 하지 않은 경우 : 변숫값을 보여주는 Variables 탭 확인
<div align="center">
<img src="https://github.com/user-attachments/assets/134c8c16-bba1-42d6-bc77-0a6dfa992db2">
</div>

   - Variables 탭의 t가 2이므로 2번쨰 테스트 케이스가 진행 중임
   - 그리고 중단점은 20번쨰 줄을 가리키고 있으므로, 이제 막 2번째 테스트를 실행하기 시작한 시점
     + 하지만 answer의 값은 0이 아닌 1327708815로, 이는 초기화 로직에 문제가 있음을 의미
     + 즉, 1번쨰 테스트 케이스에 도출한 answer의 값이 그대로 남아있다는 것

   - 변수 초기화 로직은 놓치기 쉬우며, 코드가 복잡해질수록 더욱 놓치기 쉬움
   - 또한, 변수 초기화 로직으로 테스트를 통과하지 못하는 경우도 종종 발생
   - 따라서, 실제 코딩 테스트의 2번째 테스트 케이스부터 통과되지 않으면, 모든 변수가 정상적으로 초기화되고 있는지 디버깅을 잉요해 확인하는 것도 문제 해결에 도움이 됨

3. 반복문에서 인덱스 범위 지정 오류 찾아보기
   - 인덱스 범위를 잘못 지정한 경우
<div align="center">
<img src="https://github.com/user-attachments/assets/ebaafd75-eb7b-4416-ac21-0bef9ff8aab2">
</div>

   - 종종 반복문에서 반복 범위를 잘못 지정하거나 비교 연산자를 반대로 사용하는 경우가 존재
   - 위 경우 Variables 탭에서 볼 수 있듯이 S[10000]부터 값이 모두 0 : 반복문에서 배열 S에 값에 제대로 저장되지 않음
     + 13 ~ 17번 라인을 보면 초기에 합 배열을 구하는 과정에서 반복문 범위가 10000인 것이 문제
     + 배열 A와 S의 크기가 100001이므로 반복 범위는 100000이어야 함

   - 이런 경우 외에도 배열 인덱스가 0부터 시작한다는 사실을 간과하는 경우도 존재하고, 반복문을 N까지 반복하도록 설정해야 하는데 비교 연산자를 잘못 입력해 N - 1까지 반복하도록 설정하는 경우도 존재
   - 인덱스 범위 지정 오류는 여러 형태로 발생할 수 있으므로, 반복문을 사용할 때마다 범위의 시작 인덱스를 꼼꼼하게 확인하고, 혹시 모를 입력 실수에 대비해 디버깅하는 습관을 들여야 함

4. 잘못된 변수 오류 사용 찾기
   - 출력 부분이나 로직 안에서 사용해야 하는 변수를 다른 변수와 혼동하여 잘못 사용하는 경우 존재
   - 예를 들어, 반복문에서 반복 변수를 사용해야 하는데 기준 변수를 사용하거나 변수 이름 자체가 비슷해 잘못 사용하는 경우
<div align="center">
<img src="https://github.com/user-attachments/assets/dc8a4b87-2343-487e-901e-ff751a923479">
</div>

   - t는 1이므로 1번째 테스트케이스이며, 디버깅은 26번 라인을 가리키고 있음
   - 원래 문제에서 요구한 테스트 케이스의 출력 결과는 '몇 번째 테스트 케이스의 답이 무엇이다'이므로 1 999999와 같이 출력되어야 함
     + 하지만, Console 탭을 보면 5 -187552971을 출력하는데, 이는 26번 라인에서 출력 변수가 t가 아닌 testcase로 지정했기 때문임
     
