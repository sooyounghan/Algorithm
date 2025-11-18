-----
### 문제 103번 - 다각형의 넓이 구하기 (문제 2166번)
-----
1. 문제
```
2차원 평면상에 N (3 ≤ N ≤ 10,000)개의 점으로 이뤄진 다각형이 있다.
이 다각형의 넓이를 구하는 프로그램을 작성하시오.
```

2. 입력
   - 1번쨰 줄에 N이 주어짐
   - 다음 N개의 줄에는 다각형을 이루는 순서대로 N개 점의 x, y 좌표가 주어짐
   - 좌표값은 절댓값이 100,000을 넘지 않는 정수

3. 출력
   - 1번쨰 줄에 넓이를 출력함 : 넓이를 출력할 때는 소수점 아래 둘째 자리에서 반올림 해 첫째 자리까지 출력
<div align="center">
<img src="https://github.com/user-attachments/assets/60cbd8d6-89aa-49e0-9d34-23229cb665ac">
</div>

4. 1단계 : 문제 분석하기
   - CCW는 다른 의미로 벡터의 외적값을 의미
   - 다음 그림의 CCW(A, B, C)의 절댓값은 세 점으로 이뤄진 두 벡터의 외적 크기 : 세 점을 기준으로 하는 평행사변형 넓이
<div align="center">
<img src="https://github.com/user-attachments/assets/89b2edf8-8aba-4ba3-81a2-1ae1d60e5c6b">
</div>

   - 즉, 절댓값을 2로 나누면 세 점을 꼭짓점으로 하는 삼각형의 넓이를 구할 수 있음
   - 다각형의 넓이는 결국 원점과 다른 두 점 간의 CCW로 다음과 같이 표현할 수 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/de0c3439-ab73-4ea9-8b22-a016f3520555">
</div>

   - 문제에서 점들이 순서대로 제공되는 것은 알고 있지만, 반시계 방향인지, 시계 방향인지는 알 수 없음
   - 하지만, 다른 방향의 넓이의 부호는 항상 반대이므로 원점과 순서대로 오는 두 점 간의 CCW값들의 합을 절댓값으로 변경하고 2로 나누면 다각형의 넓이가 됨
      + CCW = 벡터 외적값 = 3개의 점으로 이뤄지는 평행사변형의 넓이이므로 넓이의 값을 2로 나눠야 함

   - 원점과 다른 두 점 사이의 CCW 공식을 좀 더 단순화하면 다음과 같이 표현 가능 (두 점을 $x_{1}, y_{1}$과 $x_{2}, y_{2}$라고 가정)
<div align="center">
<img src="https://github.com/user-attachments/assets/6aa34793-1c91-4972-aeb5-b8d349fe84ad">
</div>

5. 2단계
   - 원점과 순서대로 나오는 두 점 사이의 CCW 값을 계산
<div align="center">
<img src="https://github.com/user-attachments/assets/f1a28fd5-4d6d-4103-8261-8bc1f3c2fbad">
</div>

   - 결과의 총합을 절댓값으로 변경한 후 2로 나누면 정답 도출
<div align="center">
<img src="https://github.com/user-attachments/assets/851877a3-c7d4-4388-ae4e-218fa0de3643">
</div>

6. 3단계 : 슈도코드 작성
<div align="center">
<img src="https://github.com/user-attachments/assets/d6d66120-848d-47ae-873c-adead7f21701">
</div>

7. 코드
```java
package geometry;

import java.util.Scanner;

public class PolygonArea {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int N = sc.nextInt();
        long[] x = new long[N + 1];
        long[] y = new long[N + 1];

        for (int i = 0; i < N; i++) {
            x[i] = sc.nextLong();
            y[i] = sc.nextLong();
        }

        x[N] = y[0]; // 마지막 점과 처음 점도 CCW 계산에 포함되어야 함
        y[N] = y[0];

        double result = 0;

        for (int i = 0; i < N; i++) {
            result += (x[i] * y[i + 1]) - (x[i + 1] * y[i]);
        }

        String answer = String.format("%.1f", Math.abs(result) / 2.0);
        System.out.println(answer);
    }
}
```
