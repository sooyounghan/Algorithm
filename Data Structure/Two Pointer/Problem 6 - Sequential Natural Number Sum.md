-----
### 투 포인터
-----
: 2개의 포인터로 알고리즘의 시간 복잡도를 최적화

-----
### 문제 6 - 연속된 자연수의 합 구하기 (문제 2018번)
-----
1. 문제
```
어떠한 자연수 N은 하나 이상의 연속된 자연수의 합으로 나타낼 수 있다. 예를 들어 15를 나타내는 방법은 15의 경우 7 + 8, 4 + 5 + 6, 1 + 2 + 3 + 4 + 5이다.
반면 10을 나타내는 방법은 10, 1 + 2 + 3 + 4이다. 자연수 N (1 ≤ N ≤ 10,000,000)이 주어졌을 때, N을 연속된 자연수의 합으로 나타내는 가짓수를 출력하는 프로그램을 작성하시오.
```

2. 입력 : 1번째 줄에 자연수 N이 주어짐
3. 출력 : 입력된 자연수 N을 연속된 자연수의 합으로 나타내는 가짓수를 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/a57f6b75-3589-4ad5-85b1-e3b4096c0864">
</div>

4. 1단계 : 문제 분석하기
   - 시간 복잡도 분석으로 사용할 알고리즘의 범위를 줄여야 함
   - 문제에 주어진 시간 제한은 2초 : N의 최댓값은 10,000,000으로 매우 크게 설정되어 있음
   - 이런 상황에서는 O(n log n)의 시간 복잡도 알고리즘을 사용하면 제한 시간을 초과하므로 O(n)의 시간 복잡도 알고리즘을 사용해야 함
   - 이런 경우 자주 사용하는 방법이 투 포인터
     + 연속된 자연수의 합을 구하는 문제이므로, 시작 인덱스와 종료 인덱스를 지정해 연속된 수로 표현
     + 시작 인덱스 / 종료 인덱스를 투 포인터로 지정한 후 문제 접근

2. 2단계
   - 입력받은 값을 N에 저장한 후 코드에서 사용할 변수를 모두 초기화
     + 결과 변수 count를 1로 초기화 하는 이유는 N이 15일 때 숫자 15만 뽑는 경우의 수를 미리 넣고 초기화
<div align="center">
<img src="https://github.com/user-attachments/assets/e2acf689-985b-4cb5-8d52-f94042df4a33">
</div>

   - 투 포인터 이동 원칙을 이용해 배열의 끝까지 탐색하면서 합이 N이 되는 경우의 수를 구함
     + start_index를 오른쪽으로 한 칸 이동하는 것은 연속된 자연수에서 왼쪽 값을 삭제하는 것과 효과가 같으며, end_index를 오른쪽으로 한 칸 이동하는 것은 연속된 자연수의 범위를 한 칸 더 확장하는 의미
     + 합과 N이 같을 경우에는 경우의 수를 1 증가시키고, end_index를 오른쪽으로 이동시킴
<div align="center">
<img src="https://github.com/user-attachments/assets/eb5b1047-a822-4543-8bd9-0ceaf723cf52">
</div>

   - 위 단계를 end_index가 N일 때까지 반복하되, 포인터를 이동할 때마다 현재의 총합과 N을 비교해 값이 같으면 counnt를 1만큼 증가
<div align="center">
<img src="https://github.com/user-attachments/assets/13de821c-9870-48c5-9cb4-45097fde020f">
</div>

3. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/ec74232b-ea37-4ff8-8cfd-f0902de9bb67">
</div>

4. 코드
```java
package dataStructure.twopointer;

import java.util.Scanner;

public class NaturalNumberSum {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        int N = sc.nextInt();
        int count = 1;
        int start_index = 1;
        int end_index = 1;
        int sum = 1;
        
        while(end_index != N) {
            if(sum == N) { // 현재 연속 합이 N과 같을 경우
                count++;
                end_index++;
                sum = sum + end_index;
            } else if(sum > N) { // 현재 연속 합이 N보다 더 큰 경우
                sum = sum - start_index;
                start_index++;
            } else { // 현재 연속 합이 N보다 작은 경우
                end_index++;
                sum = sum + end_index;
            }
        }

        System.out.println(count);
    }
}
```
