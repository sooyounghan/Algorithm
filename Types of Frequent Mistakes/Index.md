-----
### 인덱스에 의미 부여하여 풀어 보기
-----
1. 코딩 테스트에서 가장 많이 사용하는 자료 구조 : 배열
   - 보통 배열을 사용할 때는 인덱스로 데이터에 접근
   - 인덱스는 일반적으로 몇 번째 데이터인지 나타내는 역할
   - 하지만 상황에 따라 인덱스에 해싱(Hashing) 개념을 적용하면 단순한 위치가 아니라 특정 의미를 지닌 값으로 활용하면 문제를 더 쉽게 해결 가능

2. 그 중 인덱스를 순서가 아닌, 해당 숫잣값 자체에 의미를 부여하는 상황을 가장 자주 사용
   - 예) A[1]의 의미
     + 몇 번째 데이터인지 순서 의미 : 첫 번째 데이터 저장
     + 숫잣값으로 의미 부여 : 1이라는 값이 몇 개 인지 저장

  - 인덱스를 숫잣값으로 의미를 부여할 때 유리한 상황 가정
    + 예를 들어 1000보다 작은 자연수 10,000,000개를 1초 안에 정렬해야 하는 상황 : 일반적으로 데이터 양이 많아서 1초 안에 정렬이 어려움
    + 하지만 인덱스를 값 자체로 활용하면 계수 정렬과 같은 방식으로 제한 시간 내 정렬 가능
```java
package frequentMistake;

import java.io.*;
import java.util.StringTokenizer;

public class indexHash {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        
        int N = Integer.parseInt(br.readLine());
        int[] count = new int[1001];

        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < N; i++) {
            int number = Integer.parseInt(st.nextToken());
            count[number]++; // 인덱스에 숫자값으로 의미를 부여하여 데이터 저장
        }
        
        br.close();
        
        for (int i = 0; i < 1000; i++) {
            if(count[i] != 0) {
                for (int j = 0; j < count[i]; j++) {
                    bw.write(i + " ");
                }
            }
        }
        
        bw.flush();
        bw.close();
    }
}
```
   - 여기서 count 배열의 인덱스에 숫잣값으로 의미를 부여함으로써 정렬 속도를 크게 향상하여, 10,000,000개의 데이터를 1초 안에 정렬 가능
   - 실행 결과
```
10
10 9 8 7 6 5 4 3 2 1

1 2 3 4 5 6 7 8 9 10
```

3. 계수 정렬은 인덱스에 의미를 부여하는 좋은 예
   - 이처럼 인덱스에 의미를 부여하는 해싱 기법은 알고리즘으로 잘 체계화되어 활용되기도 함
   - 하지만, 실제 코딩 테스트에서는 요구사항에 따라 인덱스에 적절한 의미를 직접 부여하는 문제가 출제되기도 함

4. 따라서, 인덱스를 단순히 몇 번째 순서로만 생각하는 것이 아닌, 문제 상황에 따라 다양한 의미로 변환해보는 연습이 중요
