-----
### 기하 알고리즘
-----
1. 기하 알고리즘은 점, 선, 다각형, 원과 같이 각종 기하학적 도형을 다루는 알고리즘
2. 실제 코딩 테스트에서 기하 알고리즘은 CCW(Counter Clock Wise)라는 기하학적 성질을 활용
3. CCW(Counter Clock Wise)
   - 평면상의 3개의 점과 관련된 점들의 위치 관계를 판단하는 알고리즘
   - 수학적으로 벡터의 외적과 관련되었음
   - 세 점 A($X_{1}$, $Y_{1}$), B($X_{2}$, $Y_{2}$), C($X_{3}$, $Y_{3}$)라고 가정했을 때, CCW 공식
<div align="center">
<img src="https://github.com/user-attachments/assets/2444e779-bb54-4238-b024-8d78e24a77cc">
</div>

   - CCW 공식 도출 과정
     + 1번쨰 점을 뒤에 한 번 더 씀
     + 오른쪽 아래 방향 화살표 곱은 더하고, 왼쪽 아래 방향 화살표 곱은 뺌
<div align="center">
<img src="https://github.com/user-attachments/assets/d01b325b-18b2-4366-9c7f-2f9a05f53725">
</div>

   - 이렇게 세 점이 주어졌을 때, CCW 공식을 사용해 세 점에 관한 다양한 관계를 도출할 수 있음
   - CCW 공식은 기하에서 가장 기본이 됨

4. CCW 부호에 따른 3가지 의미
<div align="center">
<img src="https://github.com/user-attachments/assets/d7ca6dd4-dfde-4c1e-9f48-40d228a57cdf">
</div>

   - CCW 결과의 절댓값은 세 점으로 이뤄진 두 벡터의 외적값을 나타냄
     + 이는 CCW의 절댓값을 절반으로 나누면 세 점으로 이뤄진 삼각형의 넓이를 나타냄
     + 즉, |CCW 결과값| / 2는 세 점으로 이뤄진 삼각형의 넓이로 이해하면 됨
