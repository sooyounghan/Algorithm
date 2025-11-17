-----
### 문제 101 - 선분의 교차 여부 구하기 (문제 17387번)
-----
1. 문제
```
2차원 좌표 평면 위의 두 선분 L₁, L₂가 주어졌을 때, 두 선분이 교차하는지 아닌지 구해보자.
한 선분의 끝점이 다른 선분이나 끝 점 위에 있어도 교차한다고 판정한다.

L₁의 양 끝점은 (x₁, y₁), (x₂, y₂), L₂의 양 끝점은 (x₃, y₃), (x₄, y₄)이다.
```

2. 입력
   - 1번째 줄에 L₁의 양 끝점 x₁, y₁, x₂, y₂이 주어짐
   - 2번쨰 줄에 L₂의 양 끝점 x₃, y₃, x₄, y₄이 주어짐

3. 출력
   - L₁과 L₂가 교차하면 1, 아니면 0을 출력함 (-1,000,000 ≤ x₁, y₁, x₂, y₂, x₃, y₃, x₄, y₄ ≤ 1,000,000이며, 모두 정수)
<div align="center">
<img src="https://github.com/user-attachments/assets/3125f595-1143-4614-a9cd-3fba16e43f21">
</div>

4. 1단계 : 문제 분석하기
   - CCW의 특징을 이용하면 두 선분과 관련된 교차 여부를 구할 수 있음
   - 두 선분을 A-B, C-D라고 명명했을 때, A-B 선분을 기준으로 점 C와 D를 CCW한 값의 곱과 C-D를 기준으로 점 A와 B를 CCW한 값의 곱이 모두 음수이면 두 선분은 교차
<div align="center">
<img src="https://github.com/user-attachments/assets/6743cc69-566f-49ec-981b-85771866f389">
</div>

   - 선분 A-B를 무한대로 늘렸을 때, C, D 사이를 지나면, 두 CCW의 결과값은 항상 반대가 됨
   - 그러면 두 CCW의 결과값의 곱은 항상 음수가 되고, C-D 선분과 관련된 A, B의 CCW 결과값의 곱도 음수라면 두 선분은 교차한다고 할 수 있음
   - 두 선분이 교차하지 않으면, CCW 방향이 같을 때가 발생하고, 두 점에 관련된 CCW의 결과값의 곱이 양수
   - 선분이 겹칠 때의 겨웅
<div align="center">
<img src="https://github.com/user-attachments/assets/cc47526e-1a88-470b-aeb9-8f490aa5931b">
</div>

   - CCW의 결과값은 모두 0 : 이때는 각 선분의 min / max (x, y)값으로 겹침 여부를 판단
     + 선분이 겹치지 않는 경우, 한 선분의 min 값이 다른 선분의 max 값 보다 클 때임
     + 위의 겹치지 않는 예시에서 C-D의 min x값(점 C의 x값)이 A-B의 max x값(점 B의 x값)보다 크기 때문에 두 선분은 겹치지 않는다고 판단 가능

5. 2단계
   - 두 선분과 관련된 CCW 값의 곱을 구함
<div align="center">
<img src="https://github.com/user-attachments/assets/88cd7b96-2b00-477b-84a4-b465af28d857">
</div>

   - 결과에 따라 선분 교차 여부 확인
<div align="center">
<img src="https://github.com/user-attachments/assets/f3257b2d-70aa-4c48-987f-ad03a6a0bdfd">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/0ac3a79c-58db-4140-ad89-a2f4f6d12e3">
</div>

7. 코드
```java
package geometry;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class IntersectionLine {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());
        long x1 = Long.parseLong(st.nextToken());
        long y1 = Long.parseLong(st.nextToken());
        long x2 = Long.parseLong(st.nextToken());
        long y2 = Long.parseLong(st.nextToken());

        st = new StringTokenizer(br.readLine());
        long x3 = Long.parseLong(st.nextToken());
        long y3 = Long.parseLong(st.nextToken());
        long x4 = Long.parseLong(st.nextToken());
        long y4 = Long.parseLong(st.nextToken());

        boolean cross = isCross(x1, y1, x2, y2, x3, y3, x4, y4);

        if (cross) {
            System.out.println(1);
        } else {
            System.out.println(0);
        }
    }

    static int CCW(long x1, long y1, long x2, long y2, long x3, long y3) {
        long temp = (x1 * y2 + x2 * y3 + x3 * y1) - (x2 * y1 + x3 * y2 + x1 * y3);

        if(temp > 0) {
            return 1;
        } else if (temp < 0) {
            return -1;
        } else {
            return 0;
        }
    }

    private static boolean isCross(long x1, long y1, long x2, long y2, long x3, long y3, long x4, long y4) {
        int abc = CCW(x1, y1, x2, y2, x3, y3);
        int abd = CCW(x1, y1, x2, y2, x4, y4);
        int cda = CCW(x3, y3, x4, y4, x1, y1);
        int cdb = CCW(x3, y3, x4, y4, x2, y2);

        if (abc * abd == 0 && cda * cdb == 0) { // 선분이 일직선일 때
            return isOverlab(x1, y1, x2, y2, x3, y3, x4, y4); // 겹치는 선분인지 판별
        } else if (abc * abd <= 0 && cda * cdb <= 0) { // 선분이 교차하는 경우
            return true;
        }
        return false;
    }

    private static boolean isOverlab(long x1, long y1, long x2, long y2, long x3, long y3, long x4, long y4) {
        if((Math.min(x1, x2) <= Math.min(x3, x4)) &&
                (Math.min(x3, x4) <= Math.max(x1, x2)) &&
                (Math.min(y1, y2) <= Math.max(y3, y4)) &&
                (Math.min(y3, y4) <= Math.max(y1, y2))) {
            return true;
        }
        return false;
    }
}
```
