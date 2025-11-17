-----
### 문제 100 - 선분 방향 구하기 (문제 11758번)
-----
1. 문제
```
2차원 좌표 평면 위에 있는 점 3개 P₁, P₂, P₃이 주어진다.
P₁, P₂, P₃을 순서대로 이은 선분이 어떤 방향을 이루고 있는지 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 P₁의 (x₁, y₁)이 주어짐
   - 2번째 줄에 P₂의 (x₂, y₂)이 주어짐
   - 3번쨰 줄에 P₃의 (x₃, y₃)이 주어짐
   - -10,000 ≤ x₁, y₁, x₂, y₂, x₃, y₃ ≤ 10,000으로, 모든 좌표는 정수
   - P₁, P₂, P₃의 좌표는 서로 다름

3. 출력 : P₁, P₂, P₃을 순서대로 이은 선분이 반시계 방향이면 1, 시계 방향이면 -1, 일직선이면 0을 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/156a31c0-8a53-4105-b8f1-d909fb36a880">
</div>

4. 1단계 : 문제 분석하기 - 전형적인 CCW 문제
5. 2단계
   - P₁, P₂, P₃ 3개의 점을 입력받아 변수에 저장하고, CCW를 계산
<div align="center">
<img src="https://github.com/user-attachments/assets/7419f450-1528-4624-9380-7064d3fdb0de">
</div>

   - CCW 결과값에 따라 정답 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/78645d4c-f377-4362-8899-d4d0280aa614">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/d219765a-87ce-4935-a8cf-adcd6f275183">
</div>

7. 코드
```java
package geometry;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class CCW {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int x1 = Integer.parseInt(st.nextToken());
        int y1 = Integer.parseInt(st.nextToken());

        st = new StringTokenizer(br.readLine());
        int x2 = Integer.parseInt(st.nextToken());
        int y2 = Integer.parseInt(st.nextToken());

        st = new StringTokenizer(br.readLine());
        int x3 = Integer.parseInt(st.nextToken());
        int y3 = Integer.parseInt(st.nextToken());

        int result = (x1 * y2 + x2 * y3 + x3 * y1) - (x2 * y1 + x3 * y2 + x1 * y3);
        int ans = 0;

        if(result > 0) {
            ans = 1;
        } else if (result < 0) {
            ans = -1;
        } else {
            ans = 0;
        }

        System.out.println(ans);
    }
}
```
