-----
### 문제 32 - 원하는 정수 찾기 (문제 1920번)
-----
1. 문제
```
N개의 정수 A[1], A[2], ..., A[N]이 주어졌을 때, 이 안에 X라는 정수가 존재하는지 알아내는 프로그램을 작성하시오.
```

2. 입력
   - 1번째 줄에 자연수 N(1 ≤ N ≤ 100,000)
   - 그 다음 줄에 N개의 정수 A[1], A[2], ..., A[N]이 주어짐
   - 그 다음 줄에 M(1 ≤ M ≤ 100,000)
   - 그 다음 줄에 M개의 수들이 주어짐
   - 이 수들이 A안에 존재하는지 알아내면 되며, 모든 정수의 범위는 $-2^{31}$보다 크거나 같고, $2^{31}$보다는 작음

3. 출력 : M개의 줄에 답을 출력 (존재하면 1, 존재하지 않으면 0을 출력)
<div align="center">
<img src="https://github.com/user-attachments/assets/dad58577-9b4c-49e8-bc8a-03b97895de8c">
</div>

4. 1단계 : 문제 분석하기
   - N의 최대 범위가 100,000이므로 단순 반복문으로는 이 문제 해결 불가
   - 이진 탐색을 적용하면 O(n log n) 시간 복잡도로 해결 가능하므로 이진 탐색 적용
   - 이진 탐색을 정렬은 가정하므로 정렬 함수도 사용
     + 자바의 기본 정렬은 O(n log n)의 시간 복잡도를 가지므로 정렬을 수행해도 제한 시간을 초과하지 않음

5. 2단계
   - 탐색 데이터를 1차원 배열에 저장한 다음 저장된 배열을 정렬
<div align="center">
<img src="https://github.com/user-attachments/assets/972e58f8-eaee-480e-b8f3-764af4a40bfb">
</div>

   - X라는 정수가 존재하는지 이진 탐색을 사용해 확인
<div align="center">
<img src="https://github.com/user-attachments/assets/23965c7e-a42a-4a21-8ce7-a37673be126c">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/12e7f0d8-9500-4754-aed3-5f8c9bdd6e21">
</div>

7. 코드
```java
package search.binarysearch;

import java.util.Arrays;
import java.util.Scanner;

public class DesiredNumberSearch {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        int N = sc.nextInt();
        int[] A = new int[N];
        
        for (int i = 0; i < N; i++) {
            A[i] = sc.nextInt();
        }
        
        Arrays.sort(A);
        
        int M = sc.nextInt();
        for (int i = 0; i < M; i++) {
            boolean find = false;
            int target = sc.nextInt();
            
            // 이진 탐색 시작
            int start = 0;
            int end = A.length - 1;
            
            while (start <= end) {
                int midi = (start + end) / 2;
                int midV = A[midi];
                
                if (midV > target) {
                    end = midi - 1;
                } else if (midV < target) {
                    start = midi + 1;
                } else {
                    find = true;
                    break;
                }
            }
            
            if (find) {
                System.out.println(1);
            } else {
                System.out.println(0);
            }
        }
    }
}
```
