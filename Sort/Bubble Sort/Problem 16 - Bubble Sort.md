-----
### 문제 16 - 버블 정렬 프로그램 1 (문제 1377번)
-----
1. 문제
<div align="center">
<img src="https://github.com/user-attachments/assets/6343a76d-5c1e-45f2-bcaf-85e5129e2f03">
</div>

2. 입력
   - 1번쨰 줄에 N이 주어짐 (N는 500,000보다 작거나 같은 자연수)
   - 2번쨰 줄부터 N개의 줄에 A[1]부터 A[N]까지 1개씩 주어짐
   - A에 들어있는 수는 1,000,000보다 작거나 같은 자연수 또는 0

3. 출력 : 정답을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/c3674e34-4748-4abf-bbd2-4cc10fd348b4">
</div>

4. 1단계 : 문제 분석하기
   - 버블 정렬의 swap이 한 번도 일어나지 않은 루프가 언제인지 알아내는 문제
   - 버블 정렬 중 이중 for문 전체를 돌 때 swap이 일어나지 않았음의 의미 : 이미 모든 데이터가 정렬됐다는 것을 의미
     + 이 때는 프로세스를 바로 종료해 시간 복잡도를 줄일 수 있음
   - 하지만 이 문제는 N의 최대 범위가 500,000이므로 버블 정렬로 문제를 풀면 시간을 초과할 수 있음
   - 💡 안쪽 for 문이 몇 번 수행됐는지 구하는 다른 아이디어가 필요
     + 안쪽 루프는 1에서 n - i까지, 즉 왼쪽에서 오른쪽으로 이동하면서 swap을 수행
     + 이는 특정 데이터가 안쪽 루프에서 swap의 왼쪽으로 이동할 수 있는 최대 거리가 1이라는 뜻
     + 즉, 데이터를 정렬 전 index와 정렬 후 index를 비교해 왼쪽으로 가장 많이 이동한 값을 찾으면 이 문제 해결 가능

5. 2단계
   - 자바에서 기본적으로 제공하는 sort() 함수로 배열 정렬 : sort() 함수의 시간 복잡도는 O(n log n)
<div align="center">
<img src="https://github.com/user-attachments/assets/fa64a22d-2827-4045-9224-792280fdd588">
</div>

   - 각 데이터마다 정렬 전 index 값에서 정렬 후 index 값을 빼고 최대값을 찾음
   - 💡 그리고 swap이 일어나지 않은 반복문이 한 번 더 실행되는 것을 감안해 최댓값에 1을 더함
<div align="center">
<img src="https://github.com/user-attachments/assets/7e11fc35-e9eb-4e72-b6ae-01ba3574716c">
</div>

6. 3단계 : 슈도코드
<div align="center">
<img src="https://github.com/user-attachments/assets/65f43f3c-9e02-4509-9480-6e0329d59e3d">
</div>

7. 코드
```java
package sort.bubblesort;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class BubbleSort1 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());
        mData[] A = new mData[N];

        for(int i = 0; i < N; i++) {
            A[i] = new mData(Integer.parseInt(br.readLine()), i);
        }

        Arrays.sort(A); // A 배열 정렬(O(n log n) 시간 복잡도)

        int Max = 0;
        for(int i = 0; i < N; i++) {
            if(Max < A[i].index - i) { // 정렬 전 index - 정렬 후 index 계산의 최댓값 저장하기
                Max = A[i].index - i;
            }
        }
        System.out.println(Max + 1);
    }
}

class mData implements Comparable<mData>{
    int value;
    int index;

    public mData(int value, int index) {
        this.value = value;
        this.index = index;
    }

    @Override
    public int compareTo(mData o) { // value를 기준으로 오름차순 정렬
        return this.value - o.value;
    }
}
```
