-----
### 시간 복잡도
-----
1. 알고리즘에서 시간복잡도는 주어진 문제를 해결하기 위한 연산횟수로, 일반적으로 수행 시간은 1억 번의 연산을 1초의 시간으로 간주하여 예측
2. 시간 복잡도 정의하기
   - 빅-오메가(Ω(n)) : 최선일 떄(Best Case)의 연산 횟수를 나타낸 표기법
   - 빅-세타(θ(n)) : 보통일 때(Average Case)의 연산 횟수를 나타낸 표기법
   - 빅-오(O(n)) : 최악일 때(Worst Case)의 연산 횟수를 나타낸 표기법

3. 예) 0 ~ 99 사이의 무작윗값을 찾아 출력하는 코드
   - 빅-오메가 표기법 (Ω(n)) : 1번의 시간 복잡도
   - 빅-세타 표기법 (θ(n)) : N / 2번의 시간 복잡도
   - 빅-오 표기법 (O(n)) : N번의 시간 복잡도
   - 예제 코드
```java
package time_complexity;

public class timeComplexityExample1 {
    public static void main(String[] args) {
        // 0 ~ 99 사이의 값 무작위 선택
        
        int findNumber = (int)(Math.random() * 100);
        
        for (int i = 0; i < 100; i++) {
            if(i == findNumber) {
                System.out.println(i);
                break;
            }
        }
    }
}
```

4. 코딩 테스트에서의 시간 복잡도 사용
   - 코딩 테스트에서는 빅-오 표기법(O(n))을 기준으로 수행 시간을 계산하는 것이 좋음
   - 작성한 프로그램으로 다양한 테스트 케이스를 수행해 모든 케이스를 통과해야만 합격으로 판단하므로 시간 복잡도를 판단할 때는 최악일 때(Worst Case)를 염두에 둬야 함
   - 빅-오 표기법(O(n))으로 표현한 시간 복잡도 그래프
<div align="center">
<img src="https://github.com/user-attachments/assets/e4b3743b-9930-400d-8c26-8d26fbf9308f">
</div>

   - 각각의 시간 복잡도는 데이터 크기(N)의 증가에 따라 성능(수행 시간)이 다름
